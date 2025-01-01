# Kaggle Competition - Finetune Gemma 2 #

This repo works alongside this [kaggle notebook](https://www.kaggle.com/code/rhysie/gemma-for-american-english-crosswords), which together form a submission into the [Google - Unlock Global Communication with Gemma](https://www.kaggle.com/competitions/gemma-language-tuning/overview) competition. It contains both the notebook itself, and importantly the data generation code used to generate the [synthetic set of crossword clues](https://www.kaggle.com/datasets/rhysie/ai-generated-cross).

## Data Generation Pipeline ##
To build up a synthetic set of crossword clues and answers, we follow this process:

1. **Identify Common Words**: We take the Brown Corpus (small 1 million word corpus) from the NLTK package, and calculate the most frequently occurring words.
2. **Choose a common word**: We then select a commonly occurring word, and will ask our models to generate clues for that word.
3. **Specify a type of clue**: We want diversity in our clues, so we ask the set of models to produce a specific type of crossword clue for each word.
4. **LLM Judging**: We set up another LLM to review the three candidates and select a winning clue for fine-tuning.

### Clue Type Distribution ###
We distribute our clue types according to difficulty and style:
* Easy Clues (60% of the time): We generate a straightforward clue, e.g. "large trunk" as a clue for elephant
* Moderate Clues (15% of the time): We generate a more difficult clue, e.g. "largest land animal" is a slightly harder clue for elephant. 
* Fill in the Blank Clues (10% of the time): We generate a fill in the blank clue, e.g. "___ in the room" which requires us to realise the saying is "elephant in the room". 
* Wordplay clues (7.5% of the time): We generate a creative wordplay clue, e.g. "unforgettable one?" since elephants are known for their memory
* Example clues (7.5% of the time): We generate an "example clue", e.g. "Jumbo, perhaps", since Jumbo is an example of an elephant

### Selection Criteria ###
Our judge LLM reviews candidates according to:
* Accuracy and appropriateness
* Adherence to clue type format
* Clarity and fairness
* Avoidance of the answer in the clue
* Following crossword conventions

Appreciate that sounds like quite a complex process, so the below diagram breaks it down into steps to make it clear. We use an example which is the word "first", and leads to row 6 which we show in the dataset below the diagram:

![A step by step breakdown of the synthetic data generation process](https://github.com/rhyscook92/Kaggle-Gemma-Finetune/blob/main/images/data_generation_pipeline.PNG?raw=true)

**Figure 1:** A step by step breakdown of the synthetic data generation process.

### API Requirements: ###
Requires OpenAI, Anthropic and Google Gemini API authenticators. 