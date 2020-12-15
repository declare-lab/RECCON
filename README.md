# Recognizing Emotion Cause in Conversations

This repository contains the dataset and the pytorch implementations of the models from the paper Recognizing Emotion Cause in Conversations.

## Overview of the Task

![Alt text](RECCON.png?raw=true "Task Details")

Given an utterance U_t, labeled with emotion E_t, the task is to extract the causal spans S_c from the conversational history H_{U_t}(including utterance U_t)that sufficiently represent the causes of emotion E_t.

## Dataset

The original annotated dataset can be found in the json files in the `data\` folder. The dataset with negative examples for the Causal Span Extraction and the Causal Entailment of Emotion tasks can be found in `data\qa\` and `data\classification\` folders respectively.

### Data Format

The annotations and dialogues of the DailyDialog and IEMOCAP are available at [`data/*.json`](data/*.json).
Each instance in the JSON file is allotted one identifier (e.g. "1\_60") which is a dictionary of the following items:   

| Key                     | Value                                                                          | 
| ----------------------- |:------------------------------------------------------------------------------:| 
| `utterance`             | The text of the target utterance to classify.                                  | 
| `speaker`               | Speaker of the target utterance.                                               | 
| `context`               | List of utterances (in chronological order) preceding the target utterance.    | 
| `context_speakers`      | Respective speakers of the context utterances.                                 | 
| `sarcasm`               | Binary label for sarcasm tag.                                                  | 

Example format in JSON:

```json
{
  "1_60": {
    "utterance": "It's just a privilege to watch your mind at work.",
    "speaker": "SHELDON",
    "context": [
      "I never would have identified the fingerprints of string theory in the aftermath of the Big Bang.",
      "My apologies. What's your plan?"
    ],
    "context_speakers": [
      "LEONARD",
      "SHELDON"
    ],
    "sarcasm": true
  }
}
```

## Causal Span Extraction

We formulate the Causal Span Extraction task as a question answering task. To train RoBERTa or SpanBERT models for this task on the DailyDialog dataset use the following command:

`python train_qa.py --model [rob|span] --fold [1|2|3] --context`

Then, evlaution can be carried out on DailyDialog or IEMOCAP as follows:

`python eval_qa.py --model [rob|span] --fold [1|2|3] --context --dataset [dailydialog|iemocap]`


## Causal Entailment of Emotion

The Causal Entailment of Emotion task is formulated as a classification task. To train RoBERTa-Base or RoBERTa-Large models for this task on the DailyDialog dataset use the following command:

`python train_classification.py --model [rob|robl] --fold [1|2|3] --context`

Then, evlaution can be carried out on DailyDialog or IEMOCAP as follows:

`python eval_classification.py --model [rob|robl] --fold [1|2|3] --context --dataset [dailydialog|iemocap]`

Without context models can be trained and evaluated by removing `--context` from the above commands.
