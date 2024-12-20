# synthetic_crossword_clues

Here's how our pipeline works:
1. **Identify Common Words** We take the Brown Corpus from the NLTK package, and calculate the most frequently occuring words.
2. **Specify a type of clue** We select the type of clue we want. This can be:
    * Easy Clues (p=0.6)
    * Moderate Clues (p=0.15)
    * Fill in the Blank Clues (p=0.1)
    * Wordplay clues (p=0.075)
    * Example clues (p=0.075)
4. **Generated Candidate Clues** We then tell each frontier model both the word we need a clue for, but also the type of clue being generated.
5. **LLM Judging** We now set up another LLM, whose job is to review the three candidates, and select a winning clue that will ultimately be used in fine tuning. We review according to:
    * Accuracy and appropriateness
    * Adherence to clue type format
    * Clarity and fairness
    * Avoidance of the answer in the clue
    * Following crossword conventions
Okay, that's a lot. The below diagram breaks it down into steps though, and shows how we arrive at row 6 in the dataset, which we print below.

### API Requirements: ###
Requires OpenAI, Anthropic and Google Gemini API authenticators. 