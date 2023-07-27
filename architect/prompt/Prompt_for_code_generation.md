----
#### Prompt Engineering Guide(Web Page)
https://www.promptingguide.ai/applications/coding

#### Improving ChatGPT Prompt for Code Generation(Arxiv)
https://arxiv.org/abs/2305.08360

#### Create prompts to generate code-Vertex AI(Web Page)
https://cloud.google.com/vertex-ai/docs/generative-ai/code/code-generation-prompts

#### Prompt Engineering-Microsoft CodeX(Web Page)
https://microsoft.github.io/prompt-engineering/

#### Prompting ChatGPT for Python Code Generation: An Effective Framework(Blog)
https://artificialcorner.com/prompting-chatgpt-for-python-code-generation-an-effective-framework-e323b2d24987

#### 50 Chat GPT Prompts Every Software Developer Should Know (Web Page)
https://dev.to/hackertab_org/50-chat-gpt-prompts-every-software-developer-should-know-tested-9al

#### ChatGPT Prompts for Coding – Master Coding Faster with 200 Prompts
https://chatgptconnect.com/chatgpt-prompts-for-coding-software-developer/#code-readability-and-style


----
## Basic framwork of a prompt

+ **Instruction:** Specific tasks you want hope to execute.

+ **Context:** Background(or Context more precisely) information, which could guide LLMs respond better.

+ **Input Data:** The data that you hope LLMs to process.

+ **Output Indicator:** Data type and format hoped.

---
# General Prompt

### 1. Write clear and specific instructions

 When submitting the Prompt, the specific information of the task should be described as clearly and unambiguously as possible, including task objectives, required actions, relevant conditions, etc. 

#### System message

The system message is included at the beginning of the prompt and is used to prime the model with context, instructions, or other information relevant to your use case. You can use the system message to describe the assistant’s personality, define what the model should and shouldn’t answer, and define the format of model responses. 

#### Use delimiters

- Delimiters can be anything like: 
```
```, """, < >, `<tag> </tag>`, `:`
```

Using clear syntax for your prompt—including punctuation, headings, and section markers—helps communicate intent and often makes outputs easier to parse. 

```python
text = f"""
You should express what you want a model to do by \ 
providing instructions that are as clear and \ 
specific as you can possibly make them. \ 
This will guide the model towards the desired output, \ 
and reduce the chances of receiving irrelevant \ 
or incorrect responses. Don't confuse writing a \ 
clear prompt with writing a short prompt. \ 
In many cases, longer prompts provide more clarity \ 
and context for the model, which can lead to \ 
more detailed and relevant outputs.
"""
prompt = f"""
Summarize the text delimited by triple backticks \ 
into a single sentence.
```{text}``` """
response = get_completion(prompt)
print(response)
```

### Ask for structured output (HTML, JSON, Md Table, etc.)

When using the system message to define the model’s desired output format in your scenario, consider and include the following types of information: 

- **Define the language and syntax** of the output format. If you want the output to be machine parse-able, you may want the output to be in formats like JSON, XSON or XML.
- **Define any styling or formatting** preferences for better user or machine readability. For example, you may want relevant parts of the response to be bolded or citations to be in a specific format.

```python
prompt = f"""
Generate a list of three made-up book titles along \ 
with their authors and genres. 
Provide them in JSON format with the following keys: 
book_id, title, author, genre.
"""
response = get_completion(prompt)
print(response)
```

#### Check whether conditions are satisfied

```python
text_2 = f"""
The sun is shining brightly today, and the birds are \
singing. It's a beautiful day to go for a \ 
walk in the park. The flowers are blooming, and the \ 
trees are swaying gently in the breeze. People \ 
are out and about, enjoying the lovely weather. \ 
Some are having picnics, while others are playing \ 
games or simply relaxing on the grass. It's a \ 
perfect day to spend time outdoors and appreciate the \ 
beauty of nature.
"""
prompt = f"""
You will be provided with text delimited by triple quotes. 
If it contains a sequence of instructions, \ 
re-write those instructions in the following format:

Step 1 - ...
Step 2 - …
…
Step N - …

If the text does not contain a sequence of instructions, \ 
then simply write \"No steps provided.\"

\"\"\"{text_2}\"\"\"
"""
response = get_completion(prompt)
print("Completion for Text 2:")
print(response)
```

### Few-show learning

 A very common way to adapt language models to new tasks is to use few-shot learning. In few-shot learning a set of training examples is provided in the prompt and then the model is asked to complete one or more unfinished examples. 
 ```python
 prompt = f"""
Your task is to answer in a consistent style.

<child>: Teach me about patience.

<grandparent>: The river that carves the deepest \ 
valley flows from a modest spring; the \ 
grandest symphony originates from a single note; \ 
the most intricate tapestry begins with a solitary thread.

<child>: Teach me about resilience.
"""
response = get_completion(prompt)
print(response)
```
 
### 2. Give the model time to "think"

 + Specify the steps required to complete a task

 + Ask for output in a specified format

+  Instruct the model to work out its own solution before rushing to a conclusion
```python
prompt = f"""
Your task is to determine if the student's solution \
is correct or not.
To solve the problem do the following:
- First, work out your own solution to the problem. 
- Then compare your solution to the student's solution \ 
and evaluate if the student's solution is correct or not. 
Don't decide if the student's solution is correct until 
you have done the problem yourself.

Use the following format:
Question:
```question here
```Student's solution:
```student's solution here
```Actual solution:
```steps to work out the solution and your solution here
```Is the student's solution the same as actual solution \
just calculated:
```yes or no
```Student grade:
```correct or incorrect
```Question:
```I'm building a solar power installation and I need help \
working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations \
as a function of the number of square feet.
``` Student's solution:
```Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
```Actual solution:
"""
response = get_completion(prompt)
print(response)
```
### 3. Iterative Prompt Developmet
![[Pasted image 20230727142904.png]]

### 4. Tell LLMs what to do instead of what not to do

 ```
Another common trick when designing cues is to avoid saying what not to do, and instead say what to do. This encourages being more specific and focusing on the details that lead to a good response from the model.
```

### 5. Prompt of thought chain

```
Chain of Thinking (CoT) hints introduced in [Wei et al. (2022) (opens in a new tab)](https://arxiv.org/abs/2201.11903) through intermediate reasoning steps Sophisticated reasoning capabilities are achieved, enabling more complex tasks to be performed in this way.
```
---
# Prompt designed for programming

### 1. Create Project

+ **Requirement analysis** Interpret the following project requirements and suggest a high-level architecture or design: `[requirements description]`.

+ **Project planning** Develop a high-level project plan that includes tasks, resources, and timelines for a project with the following objectives: `[project objectives]`.

### 2. Code Design

+ **Code scaffolding** Generate a `[language]` code template for a `[type of application or service]` that follows best practices: `[application or service description]`.

+ **Code Generation** Generate a boilerplate `[language]` code for a `[class/module/component]` named `[name]` with the following functionality: `[functionality description]`.

+ **Code Completion** In `[language]`, complete the following code snippet that initializes a `[data structure]` with `[values]`: `[code snippet]`.

+ **Code Review** Review the following `[language]` code for best practices and suggest improvements: `[code snippet]`.

+ **Integration and interoperability** Suggest a strategy for integrating the given `[language]` code with `[external system or API]`: `[code snippet]`.

+ **PPA optimize** Propose changes to the given `[language]` code to optimize its performance, power and area: `[code snippet]`.

### 3. Code Verification

+ **Bug Detection** Analyze the given `[language]` code and suggest improvements to prevent `[error type]`: `[code snippet]`.

+ **Automated Testing** Generate test cases for the following `[language]` function based on the input parameters and expected output: `[function signature]`.

+ **Issue tracking and resolution** Suggest potential solutions for the following reported issue: `[issue description]`.

### 4. User Documentation

+ **API documentation generation** Document the expected input and output for the given `[language]` function: `[code snippet]`.

+ **Technical documentation** Create a user guide for the given `[software or tool]` that covers installation, configuration, and basic usage.

+ **Code visualization** Visualize the call graph or dependencies of the following `[language]` code: `[code snippet]`.

+ **Data visualization** Generate a scatter plot that demonstrates the relationship between the following two variables: `[variable 1]` and `[variable 2]`.

+ **Code analytics** Generate a report on the complexity and maintainability of the following codebase: `[repository URL or codebase description]`.

ref:

https://dev.to/hackertab_org/50-chat-gpt-prompts-every-software-developer-should-know-tested-9al

https://chatgptconnect.com/chatgpt-prompts-for-coding-software-developer/#code-analytics