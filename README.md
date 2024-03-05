# Dental Diagnosis Classification Project

## Project Overview:

Our project aims to revolutionize the dental diagnosis process by leveraging advanced natural language processing (NLP) techniques. We propose a solution that utilizes a fine-tuned pre-trained language model (BERT) to analyze patient text descriptions and provide:

- Accurate classifications of dental symptoms and concerns.
- Detailed explanations to improve user understanding.

This project aligns with the growing demand for telemedicine and digital health solutions, offering an innovative approach to:

- Streamline dental care delivery.
- Improve patient outcomes by enabling earlier diagnoses and interventions.

## Problem Statement:

Traditional dental diagnosis methods rely heavily on:

- Manual examination.
- Patient reporting, which can be subjective and lead to:
  - Misdiagnosis.
  - Delays in treatment.

Additionally, accessing timely dental care can be challenging due to:

- Limited access to dentists, especially in underserved areas.
- Long wait times for appointments.

There is a critical need for an:

- Automated.
- Accessible.
- Accurate dental diagnosis solution to:
  - Cater to diverse patient populations.
  - Improve overall oral health outcomes.

## Features:

- Fine-tuned BERT model for sequence classification.
- Accurate diagnosis predictions based on patient-provided text descriptions.
- Detailed explanations for each diagnosis, enhancing user comprehension.

##Architecture :

### Client-side interface:
- User-friendly interface for patients to input their dental symptoms or concerns.
- Sends the input text to the server for processing.

### Server-side backend:
- Receives the input text from the client.
- Utilizes the fine-tuned NLP model to analyze the text and predict the diagnosis.
- Provides detailed explanations for each diagnosis.
- Returns the diagnosis prediction and explanation to the client for display.

### worklfow digaram

insert image

## Sample Data Example:

| Text Description                                                     | Diagnosis   |
|----------------------------------------------------------------------|-------------|
| "I have a sharp pain in my upper right tooth, especially when I bite down." | Caries      |
| "My gums are bleeding and swollen, especially when I brush my teeth."      | Gingivitis  |
| "I want to whiten my teeth for cosmetic reasons."                          | Cosmetic    |

## Technical Stack (Optional):

- Programming Language: Python 
- Libraries: PyTorch 

## Challenges:

- Data Collection: Obtaining a comprehensive dataset with diverse patient data requires collaboration with dental professionals and addressing privacy concerns.
- Model Fine-Tuning: Fine-tuning the BERT model requires expertise in NLP and experimentation to achieve optimal performance.
- User Interface Design: Creating an interface that effectively communicates diagnoses and explanations to users with varying dental knowledge is crucial.

## Scope:

This project focuses on developing a robust system for dental diagnosis classification. Future exploration includes:

- Real-time symptom tracking.
- Integration with electronic health records (EHRs).
- Multilingual support to enhance accessibility.

## Conclusion:

This project has the potential to significantly improve the dental diagnosis process, offering a more accessible and accurate solution for patients and healthcare providers. By addressing the identified challenges and leveraging the proposed technical approach, we aim to contribute to advancements in telemedicine and oral healthcare.

## Sample Code Example:

```python
import pandas as pd
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

# Sample dental diagnosis data 

data = pd.DataFrame({
    "text": [
        "The patient has visible carious lesions on the upper left central incisor.",
        "The dentist recommended a filling for the decayed tooth due to caries progression.",
        "The patient is concerned about gum bleeding and redness.",  
        "The wisdom teeth are causing discomfort and might require extraction.", 
        "The patient wants to whiten their teeth for cosmetic reasons.",  
    ],
    "label": [
        "caries",
        "healthy",
        "gingivitis",  
        "wisdom teeth",  
        "cosmetic", 
    ]  
})

# Preprocess text data (as before)
data["text"] = data["text"].str.lower()
data["text"] = data["text"].str.replace("[^a-zA-Z0-9\s]", "", regex=True)  # Remove punctuation

# Pre-trained LLM 

model_name = "bert-base-uncased"  
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=len(set(data["label"])))

# Function to diagnose using the LLM
def diagnose(text):
    encoded_text = tokenizer(text, return_tensors="pt")
    with torch.no_grad():
        outputs = model(**encoded_text)
        predictions = torch.argmax(outputs.logits, dim=-1)
    return data["label"].iloc[predictions.item()]

# Improved explanations with additional information and avoiding medical jargon (replace with factual sources)

explanations = {
    "caries": "Cavities, also known as tooth decay, occur when plaque buildup weakens tooth enamel. It can cause pain, sensitivity, and tooth loss. Early detection and treatment are crucial.",
    "healthy": "The provided text suggests good oral health. Regular check-ups are still recommended for preventative care.",
    "gingivitis": "This indicates early-stage gum inflammation (gingivitis) characterized by bleeding and redness. Early treatment with improved oral hygiene and professional cleaning can prevent progression to periodontitis.",
    "wisdom teeth": "Wisdom teeth are the third molars that may erupt partially or fully, causing discomfort or impaction. Consult your dentist for evaluation and potential extraction.",
    "cosmetic": "This text doesn't suggest dental issues, but expresses interest in cosmetic procedures like teeth whitening. Consult a dentist for safe and appropriate options."
}

# User interaction 

while True:
    user_input = input("Enter patient text (or 'quit' to exit): ")
    if user_input.lower() == "quit":
        break

    diagnosis = diagnose(user_input)
    explanation = explanations.get(diagnosis, "Diagnosis explanation unavailable.")

    print(f"Diagnosis: {diagnosis}")
    print(f"Explanation: {explanation}")
