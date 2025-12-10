# 🧮 MathLABS: Visual Mathematical Reasoning Dataset

MathLABS is a collaborative dataset project exploring **visual mathematical reasoning** in modern LLMs and VLMs.  
The goal is to benchmark model reasoning on **small-data visual math problems** with structured, schema-aligned questions.

---

## Repository Structure
```
mathlabs/
│
├── dataset/
│ ├── baseline.json #Intial pool of Questions
│ └── images/ #Intial Images in baseline.json
│ └── extractor.py
│ └── generator.py
│
├── model_eval/
│ ├── prompts.md 
│ ├── evaluator.py # automated evaluator
│ ├── sample_evaluations.json
├── streamlit/ # Dashboard
│ ├──pages/ # Code for each of the streamlit pages
│
├── docs/
│ ├── schema_description.md
│ ├── design_notes.md
│ └── roadmap.md

```


---

## Workflow

### **Phase 1: Dataset**
All team members collaborate to create a robust baseline in ```baseline.json```, then split tasks for efficiency:

| Step | Description |
|------|-------------|
| **Baseline** | Initial question pool (~100 questions) across subfields (Algebra, Geometry, Probability, etc.)|
| **Extraction** | Extract diagrams, OCR text, and structured representations|
| **Generation** | LLM-assisted synthetic question creation|
| **Formatting** | Schema normalization, MSC2020 classification, difficulty tagging|
| **Validation** | Auto + manual verification of answers, reasoning, hints|

---

### **Phase 2: Model & Evaluation**
Use validated dataset to benchmark LLMs or other models:

| Step | Description | Output |
|------|-------------|--------|
| **Benchmarking** | Few-shot, zero-shot, or visual reasoning tasks | `model_eval/results/` |
| **Metric Analysis** | Compute accuracy, reasoning correctness, symbolic validation | `model_eval/metrics/` |
| **Visualization** | Graphs and summary analysis | `model_eval/analysis/` |

---

### **Phase 3: Scaling & Dashboard**

We set up the database on  MongoDB. Additionally all the images required for generating new questions or that were extracted from books were uploaded to HuggingFace


## Research Focus
- Reasoning with **small-data**  
- **Visual + symbolic** math problem understanding  
- **Few-shot and zero-shot** evaluation of LLMs  
- Dataset structured using **MSC2020 codes** for subfield/topic classification

---

## JSON Schema Example

```json
{
  "schema_version": "mcq-1.0",
  "problem_id": "XX-YYY",
  "question_type": "multiple_choice", 
  "source": {
    "type": "extract|generation",
    "book_title": "Title_of_the_Book",
    "authors": ["Author_1_Name", "Author_2_Name", "..."],
    "edition": 1,
    "chapter": 1,
    "page": 111
  },
  "subfield": ["XX"],
  "topic": [
	  "topic_1",
    "topic_2",
    "..."
    ],
  "gradelevel": ["College-level|High-school-level|Graduate-level|Above-graduate"],
  "statement": "Statement-of-the-prompt, make sure to use $...$ or $$...$$ to wrap around $$\\LaTeX$$ expressions.",
  "diagram_data": {
    "type": "formula|image|table|...",
    "image_path": "images/XX-YYY.png"
  },

  "choices": [
    { "id": "A", 
	    "text": "Choice_A_text"
	  },
    { "id": "B", 
	    "text": "$Choice_B_text$" 
	  },
    { "id": "C", 
	    "text": "$Choice_C_text$" 
	  },
    { "id": "D", 
	    "text": "$Choice_D_text$" 
	  }
  ],

  "answer": {
    "correct_ids": ["A"],                  
    "explanation": "Explanation-of-the-answer, make sure to use $...$ or $$...$$ to wrap around $$\\LaTeX$$ expressions.",
    "distractor_rationales": {             // optional: discussion why the distractors are wrong
      "B": "Explanation to why B is wrong.",
      "C": "Explanation to why C is wrong.",
      "D": "Explanation to why D is wrong."
    },
  },

  "evaluation": {
    "scoring": { "type": "all_or_nothing", "points": 1 },
    "allow_partial_credit": false|true          // may be true for MCQs allowing more than one answers
  },

  "randomization": {
    "shuffle_choices": true|false,               // whether or not some choicees should be shuffled
    "lock_ids": [],                        // questions that require to be locked in place
    "group_shuffle": []                    // question ids that should be shuffled together (that they are similar)
  "hints": 
  [
		"No.1 Hint",
		"No.2 Hint",
		"..."
   ],
  "difficulty": "hard|medium|easy",
  "bloom_taxonomy": ["Create", "Evaluate", "Analyze", "Apply", "Comprehend", "Remember"] // due to the nature of MCQs, the Create type is going to be very hard to achieve
  "validation_status": "unverified",
  "flags": []
}

```


Team: MathLABS -> (L)ucas Yao, (A)khilesh Vangala, (B)ruce Zhang, (S)ahil Parupudi; NYU | CDS
