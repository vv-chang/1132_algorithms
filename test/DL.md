# Deep Learning #1 Report: Reasoning LLM - Step 0 - SFT

> 313551017 / 張若薇

本次作業報告將分成三大部分：**選定大型語言模型、Fine-tune語言模型、語言模型推論及預測**，並在內容中詳細記錄其技術、步驟、實驗與訓練過程，最後則為本次作業之心得。

## 一、選定大型語言模型
本次作業要使用中國釋出的模型，並在本地端用自己的機器進行訓練與推論，中國的語言模型包含Qwen, QWQ, DeepSeek-R1等，經過查詢與比較後，發現DeepSeek-R1使用以中文為主的百科等文本進行大規模訓練，相較於Qwen等模型偏向口語對話與指令生成，對長篇文本與中文選擇題語言邏輯理解表現更好，且由於中國本身政策限制，語言模型的回覆也會受影響，例如Qwen就有明確的過濾機制，故選用DeepSeek系列語言模型。

### 技術:
DeepSeek-R1-Distill-Llama-8B是由中國DeepSeek團隊自行訓練的開源大型模型，以下為二點技術分析：
#### Transformer架構
DeepSeek-R1模型採用Meta發表的LLaMA架構為基礎，在Transformer架構上其採用的Decoder-only Transformer，屬於GPT系列架構，適用於文字生成任務。
#### Knowledge Distillation 知識蒸餾
DeepSeek-R1並非由原始LLaMA模型微調而來，而是先從頭訓練出DeepSeek-R1-67B，作為高性能的教師模型，再透過知識蒸餾技術將教師模型的知識壓縮為參數較小的DeepSeek-R1-Distill-Llama-8B學生模型，保留大型模型的推理與語言理解能力，同時大幅減少記憶體與運算成本。
> 開源模型架構：LLaMA ➔ 教師模型：DeepSeek-R1-67B ➔ 學生模型：DeepSeek-R1-8B

### 步驟:
蒸餾的語言模型更適合於本地端訓練與應用，本次作業，最終選擇使用預訓練語言模型[DeepSeek-R1-Distill-Llama-8](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-8B)作為本地端預測模型使用。
* 載入本地語言模型或已訓練好之模型、checkpoint
* 啟用4-bit量化模式以節省VRAM
```
model_path = "C:/Users/ruowe/Desktop/LLM/deepseek_8b"

model = AutoModelForCausalLM.from_pretrained(
    model_path,
    device_map="auto",
    load_in_4bit=True,
    torch_dtype=torch.float16
)
```

### 實驗:
除了使用DeepSeek-R1-Distill-Llama-8，一開始嘗試使用deepseek-llm-7b-base測試預測效果，此模型為非蒸餾過的一般基礎模型，也是R1前期的模型，經相同訓練後，實測預測結果，發現效果較DeepSeek-R1-Distill-Llama-8差一點。

使用DeepSeek-R1-Distill-Llama-8
![螢幕擷取畫面 2025-04-13 230456](https://github.com/user-attachments/assets/1737fbb8-226e-487a-9cc0-fa68d316a0b7)

使用deepseek-llm-7b-base
![螢幕擷取畫面 2025-04-13 230344](https://github.com/user-attachments/assets/bcf6e6a9-d9b6-4c63-90ad-84674ef3a9d7)


### 訓練過程:
直接使用DeepSeek-R1-Distill-Llama-8進行預測
![螢幕擷取畫面 2025-04-13 231249](https://github.com/user-attachments/assets/aad51e7c-5f01-4b77-80d7-c757b4310e27)

從Hugginhg Face將DeepSeek開源語言模型下載至本地端
![螢幕擷取畫面 2025-04-13 231249](https://github.com/user-attachments/assets/2ec411d7-55bb-40d3-816a-d54bf785e97b)

為了蹤訓練過程中的參數變化與模型效能，使用WandB工具，進行訓練歷程的可視化與日誌記錄。
![螢幕擷取畫面 2025-04-13 231454](https://github.com/user-attachments/assets/d00bbd9c-80ea-4739-b07d-8f87b566fdd7)
![螢幕擷取畫面 2025-04-13 231550](https://github.com/user-attachments/assets/3011c866-1d8a-4754-a719-e1b656b9faf5)


➡️在本地端載入DeepSeek-R1-Distill-Llama-8B模型，並進行Supervised Fine-Tuning，結合LoRA架構進行微調。整體架構支援在單張RTX 4090 GPU完成訓練，訓練中亦整合驗證機制（Validation）於每個 epoch後自動執行效能評估。

## 二、Fine-tune語言模型
選定採用的語言模型後，將針對此模型進行fine-tuning，這裡使用LoRA（Low-Rank Adaptation)執行SFT(Supervised Fine-Tuning）訓練，實現中文選擇題語言模型任務。
### 技術:
#### SFT(Supervised Fine-Tuning）監督微調
SFT是基於「有標註的資料」進行語言模型微調，讓預訓練模型學會特定任務格式與目標行為，例如回答問題、分類、摘要、或本次作業中的中文單選題推論。

* 準備標註資料：每筆資料包含明確的輸入（Prompt）與對應的正確輸出（Answer）
* 建構訓練格式：將資料轉為類似對話、問答或指令的格式
* 餵入模型進行訓練：透過CrossEntropy，讓模型學會產生與標準答案一致的輸出
* 驗證模型表現：透過驗證集追蹤loss或 accuracy，觀察是否過擬合或學習不足

➡️ 本作業中使用LoRA技術實現SFT

LoRA使用PEFT（Parameter-Efficient Fine-Tuning）參數高效微調方法，透過調整模型的一小部分參數，完成微調，相較Full Fine-Tuning訓練成本低、記憶體佔用小。

### 步驟:
**1. 設定 LoRA Fine-Tune 模組**
* 使用PEFT套件的`LoraConfig`指定微調設定
* 微調Transformer的Attention機制`q_proj`(Query Projection)和`v_proj`（Value Projection）模組

```python
model = prepare_model_for_kbit_training(model)

lora_config = LoraConfig(
    r=8,
    lora_alpha=16,
    target_modules=["q_proj", "v_proj"],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)
model = get_peft_model(model, lora_config)
```
**2. 載入測試集與預處理資料**
* 讀入訓練資料集train.csv與自行延伸的training data
* 建立每題prompt將題目與選項整合成語言模型格式
* 將訓練集資料切成訓練集(85%)/驗證集(15%)，於每個epoch結束後進行模型效能評估
* 對每筆資料進行 Tokenizer 編碼
```python
df = pd.read_csv(train_path, encoding="utf-8")
df = df.dropna()
df["prompt"] = df.apply(lambda row:
    f"題目：{row['Question']}\nA. {row['Option A']}\nB. {row['Option B']}\nC. {row['Option C']}\nD. {row['Option D']}\n請選出最符合國際觀點的選項。\n答：{row['Answer']}",
    axis=1)

train_df, val_df = train_test_split(df, test_size=0.15, random_state=42)
train_dataset = Dataset.from_pandas(train_df[["prompt"]])
val_dataset = Dataset.from_pandas(val_df[["prompt"]])

def tokenize_function(example):
    return tokenizer(
        example["prompt"],
        padding="max_length",
        truncation=True,
        max_length=256,
        return_tensors="pt"
    )

tokenized_train = train_dataset.map(tokenize_function, batched=True)
tokenized_val = val_dataset.map(tokenize_function, batched=True)

```

**3. 設定Trainer與訓練參數**
* 使用Hugging Face`Trainer`執行SFT
* 設定訓練參數：`epochs`、`batch size`、梯度累積、學習率等
* 每完成一輪`epoch`儲存為checkpoint(中斷時可接續執行)

```python
training_args = TrainingArguments(
    output_dir=output_dir,
    per_device_train_batch_size=2,
    per_device_eval_batch_size=2,
    gradient_accumulation_steps=4,
    num_train_epochs=3,
    logging_steps=10,
    evaluation_strategy="epoch",
    save_strategy="epoch",
    save_total_limit=2,
    fp16=True,
    learning_rate=2e-4,
    warmup_ratio=0.03,
    lr_scheduler_type="cosine",
    report_to="wandb",
    logging_dir=f"{output_dir}/logs",
)

data_collator = DataCollatorForLanguageModeling(tokenizer, mlm=False)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=tokenized_train,
    eval_dataset=tokenized_val,
    tokenizer=tokenizer,
    data_collator=data_collator,
)
```

**4. 執行訓練與儲存模型**
* 執行`.train()`開始訓練，完成後儲存模型至指定資料夾

```python
trainer.train()
trainer.save_model(f"{output_dir}/extended_model")
```
### 實驗:
延伸測試集
(1) DPO訓練集
DPO訓練資料是由兩個回答版本組成：偏好的回答（chosen）與不偏好的回答（rejected），模型學習偏向選擇使用者偏好的回答
將train.csv題目改寫為此DPO格式
![image](https://github.com/user-attachments/assets/fd93f834-2792-4af9-948a-c2d38040983f)

(2) GPT-4 從train.csv延伸，作為新的traing data
(3) GPT-4 從test.csv延伸，作為新的traing data
根據原始題目中的四個選項延伸問題，並用GPT-4作答提供正確答案，作為延伸訓練資料
![image](https://github.com/user-attachments/assets/807d7a0e-a703-4b1b-8c50-1a54a3e3bf75)
![image](https://github.com/user-attachments/assets/b9739f0b-c916-4023-92b2-edb6ae3066d4)
![image](https://github.com/user-attachments/assets/9cb25cae-104a-4f9f-94e1-d6470c7281e3)
![image](https://github.com/user-attachments/assets/283adcc0-52ca-4dd9-bed6-dc89ecddedf9)

➡️ 無法確定原始test對應的答案標準為何，GPT生成的答案也不一定符合此標準作為正確解答，依照再訓練結果有提升一點精準度，但沒有大幅度提升

### 訓練過程:
調整不同的steps(10-->100)
![螢幕擷取畫面 2025-04-13 232836](https://github.com/user-attachments/assets/d92f3566-aedf-4a74-ab4f-70e04990934d)

加入valid 驗證集
![螢幕擷取畫面 2025-04-14 003555](https://github.com/user-attachments/assets/0cb7606e-b83c-43b9-ac3e-38ff2e52fecd)


調整不同的epoch
![螢幕擷取畫面 2025-04-13 232937](https://github.com/user-attachments/assets/0a487e22-7368-4552-bdba-62fe2ed84516)

➡️ 發現3輪效果與6-8輪效果無太大差異，後續訓練多以3輪為主
## 三、語言模型推論及預測
為了提升語言模型在中文選擇題推理任務中的準確性與一致，使用 Prompt Engineering 設計明確的輸入格式，引導模型理解題目並回答，有效控制模型僅輸出A、B、C或D代號選項。

### 技術:
#### Prompt Engineering 提示工程
在中文選擇題推論任務中，我使用Instruction+Few-shot Prompt的提示策略，設計包含語氣判斷原則與答案格式限制的輸入，並加入一筆具參考意義的示範題，幫助語言模型學習如何依據語意傾向與事實邏輯選出最符合國際觀點的選項。

### 步驟:
1. 載入模型
* 載入已經LoRA + SFT微調完成的模型
* 啟用4-bit量化及初始化 tokenizer
```python
model_path = "./sft_output/continued_model/extended_model"

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_compute_dtype=torch.float16,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_quant_type="nf4",
)

tokenizer = AutoTokenizer.from_pretrained(model_path, use_fast=True)
tokenizer.pad_token = tokenizer.eos_token

model = AutoModelForCausalLM.from_pretrained(
    model_path,
    device_map="auto",
    torch_dtype=torch.float16,
    quantization_config=bnb_config
)
model.eval()
```

2. 載入預測資料集
* 清理test-v2.csv格式(" Question"前有空格)
```python
test_df = pd.read_csv("test-v2.csv")
test_df.columns = test_df.columns.str.strip()
```

3. 訂定Prompt及Few-shot範例
* 使用Prompt Engineering設計清楚的任務指令
* 從train data 選擇一題有正解的題目，作為few-shot範例，幫助模型學習要怎麼回答
* 引導模型回答時只輸出單一選項A、B、C或D

```python
few_shot_example = """題目：中國《國家安全法》中關於新疆地區安全管理的規定主要受到哪些國際批評？
A. 侵犯人權
B. 促進穩定
C. 加強國際合作
D. 保障宗教自由
正確答案：A
"""

prompt_template = """你是一位熟悉國際政治與人權議題的分析專家，請根據以下題目內容，判斷最符合國際視角的正確選項。

請你只回答選項代號（A、B、C 或 D），不要多餘說明。

{few_shot}

題目：{question}
A. {a}
B. {b}
C. {c}
D. {d}
"""
```
4. 逐題推論並回覆正確選項
* 使用自訓練之`model.generate()`生成回應
* 從回應文字中找出A、B、C或D文字作為預測答案
* 將每筆推論輸出為預測答案，儲存成`.csv`格式檔案
```python
submission = []

for _, row in tqdm(test_df.iterrows(), total=len(test_df), desc="推論中"):
    prompt = prompt_template.format(
        few_shot=few_shot_example,
        question=row["Question"],
        a=row["Option A"],
        b=row["Option B"],
        c=row["Option C"],
        d=row["Option D"]
    )
    inputs = tokenizer(prompt, return_tensors="pt").to(model.device)

    with torch.no_grad():
        outputs = model.generate(
            **inputs,
            max_new_tokens=10,
            temperature=0.7,
            do_sample=False,
            pad_token_id=tokenizer.eos_token_id
        )

    decoded = tokenizer.decode(outputs[0], skip_special_tokens=True).strip()
    choice = "A"
    for ch in reversed(decoded):
        if ch in ["A", "B", "C", "D"]:
            choice = ch
            break

    submission.append({"ID": row["ID"], "Answer": choice})
    
    submission_df = pd.DataFrame(submission)
submission_df.to_csv("submission.csv", index=False)
print("已儲存 submission.csv，共", len(submission_df), "筆答案")
```
### 實驗與訓練過程

不同的prompt確實會影響預測的答案結果
![image](https://github.com/user-attachments/assets/df4f76b1-211e-4ad8-931d-9ca550c91aa8)

使用過長的prompt，可能反而影響推論結果，未能精準分析，直接全部回覆預設答案A
![image](https://github.com/user-attachments/assets/4bcb4efa-7c70-405e-a5b8-3961dbe1ac78)
![image](https://github.com/user-attachments/assets/2749839a-3596-45cc-ac9e-1a6971a252ef)

Few-Shot範例最多提供1-2題，過多也會造成模型預測變差，或是思考時間過長
![image](https://github.com/user-attachments/assets/61a0a835-158c-4fb0-be78-557f0667f825)


## 心得報告
在本次作業中，我實作了監督式微調的完整流程，針對一個中文單選題資料集，在本地端使用大型語言模型進行訓練與推論。首先，我使用的是中國語境下釋出的大型模型，並透過 SFT 方式餵入具有標準答案的訓練資料（train.csv），針對政治、人權、歷史等領域進行微調訓練。在此過程中，我發現fine-tuning並非只是技術性操作，還很大程度的涉及語意辨識、資料品質與模型輸出行為的多項因素影響。

在測試資料的推論階段，我特別觀察了 訓練資料與測試資料之間的語意相似性與主題一致性。實驗中發現，若兩者主題高度相符，模型通常表現良好；但若題目偏離訓練主題、複選、或語意模糊，或是定義不明確時，則容易出現偏差答案，甚至連用工人智慧都無法辨識正確答案。
例如：中國的社會運動是否能夠自由發聲？Ａ.能夠自由發聲 B. 受到嚴格限制 C. 部分地區能自由發聲 D. 社會運動受到高度控制
這類題目實際上 可能存在複選空間，且根據不同資料來源、國際視角或中國官方立場，會有完全不同的詮釋方式，進而影響模型學習與作答準確性。這也包含了，當時在生成題目時，每個題目的選項設計及答案是否有一樣的標準。也因此，我認為這次作業模型的訓練及預測並沒有達到很好的精準度。

另外，過程中我也嘗試使用 unsloth + LoRA 進行加速微調，預期可以有效減少記憶體佔用並提高效率，但實際操作時，卻遇到了 CUDA 驅動與套件版本不相容的問題，導致整個訓練環境壞掉，最終決定退回較為穩定的 Hugging Face + LoRA 架構繼續訓練。這次經驗提醒我，實務操作中的環境建構與套件相容性與版本匹配在安裝時要特別注意。相對於直接使用原始deepseek模型進行預測，fine-tune後的模型確實有提升精準度，但效果仍有限，本次作業
讓我對於fine-tune大型語言模型也有初步實作學習。
