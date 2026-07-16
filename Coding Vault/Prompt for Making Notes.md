# From raw text: 

```
Act as an expert technical writer and senior software engineer. I am going to provide you with a raw transcript/text dump from a programming course. I want you to synthesize this text into a comprehensive, highly detailed set of notes formatted for Obsidian (Markdown).

CRITICAL CONTENT RULES - DO NOT OVER-COMPRESS:

Preserve Conceptual Depth: Do not take away the "why". If the text explains the underlying memory behavior, bit sizes, runtime mechanics, or specific gotchas, you must include that explanation.

Assignments: If some content is shows as assignments, note the steps or learnings from them so that the reader can understand how and/or why the task is done, with sufficient context throughout the entire notes. 

Keep Practical Advice: Retain any developer tips, rules of thumb, or explanations of why one method is preferred over another (e.g., avoiding type mismatch pain).

Preserve Syntax Rules: Keep all explicit mentions of strict syntax behaviors (e.g., brace placement, automatic spacing in print functions, concatenation rules).

Include Code Examples: Every single concept mentioned must be accompanied by a clean, relevant code block. If the text compares a "bad way" vs a "good way" (like nested if-statements vs guard clauses), show the side-by-side code comparison.

Summarize Only Redundancy: You may reorder the content for better logical flow and remove repetitive fluff, but never remove distinct concepts or meaningful examples.

FORMATTING RULES:

Tone: Formal, structured, and educational. Remove the casual/chatty tone of the original transcript.

Structure: Make heavy use of Markdown formatting. Use ## and ### headings to create a clean hierarchy. Use bullet points and bold text (**) to emphasize key terms.

Code Blocks: Ensure all code is enclosed in the proper language-specific backticks (e.g., go ).

Direct Output: Output the formatted markdown in a copyable code block of markdown so I can copy it directly and paste it straight into Obsidian. Note that sometimes, ending code markers inside the external markdown can end this external markdown. Ensure this doesn't happen and I get a single copyable block. It might be necessary to wrap the output inside a four-backtick block so that the internal snippets don't break the formatting.

Take your time to ensure accuracy and completeness. Quality and depth are my highest priorities.

Here is the course text:
```

---

# From Youtube Videos:

```
You are an expert technical writer, software engineer, educator, and curriculum designer.

I will provide one or more YouTube video URLs.

Your task is to extract the knowledge being taught and transform it into comprehensive Obsidian-ready Markdown notes in a 4 backtick syntax so that it can be copied easily.

# PRIMARY OBJECTIVE

The output should read like a high-quality textbook chapter or engineering reference.

The notes must teach the subject matter directly.

Do NOT describe:

- what the instructor does
    
- what the instructor clicks
    
- what the instructor types
    
- what appears at specific timestamps
    
- how the video is structured
    

The reader should be able to learn the topic without knowing a video ever existed.

# KNOWLEDGE EXTRACTION MODE

Treat the video as a source of information rather than something to summarize.

Convert demonstrations, explanations, diagrams, examples, code walkthroughs, and discussions into clear teaching material.

Replace:

"The instructor demonstrates..."

with:

"This mechanism works because..."

Replace:

"The instructor shows..."

with:

"The concept is..."

Focus entirely on:

- What is being taught
    
- Why it matters
    
- How it works
    
- When it should be used
    
- Tradeoffs
    
- Pitfalls
    
- Best practices
    

# DEPTH REQUIREMENTS

Preserve all meaningful technical depth.

Include:

- Internal mechanics
    
- Runtime behavior
    
- Memory behavior
    
- Compiler behavior
    
- Type system details
    
- Architecture discussions
    
- Design decisions
    
- Tradeoffs
    
- Performance implications
    
- Edge cases
    
- Common mistakes
    

Never oversimplify concepts.

# CONCEPT ORGANIZATION

Organize notes by topic rather than by video order whenever possible.

For every major concept include:

## What It Is

Definition.

## Why It Exists

Problem it solves.

## How It Works

Internal mechanics.

## Example

Code examples.

## Advantages

Benefits.

## Limitations

Tradeoffs and drawbacks.

## Common Pitfalls

Mistakes developers make.

# CODE REQUIREMENTS

Every important programming concept should include examples.

Use clean and idiomatic code.

If multiple approaches are discussed:

- show each approach
    
- compare them
    
- explain why one might be preferred
    

# VISUAL INFORMATION

Extract useful knowledge from:

- diagrams
    
- architecture drawings
    
- whiteboards
    
- slides
    
- terminal output
    
- code demonstrations
    

Convert visual information into teaching material.

Do not describe the visual itself unless necessary.

Instead, explain the concept represented by the visual.

# ASSIGNMENTS AND EXERCISES

If exercises are presented:

Include:

## Exercise

### Goal

What concept it teaches.

### Steps

How to complete it.

### Expected Result

What should happen.

### Key Learning

Why the exercise matters.

# OUTPUT FORMAT

Generate Obsidian-ready Markdown.

Use:

- clear heading hierarchy
    
- bullet points
    
- tables when useful
    
- callouts for important information
    

# QUALITY STANDARD

The final notes should resemble a professionally written engineering handbook, technical documentation page, or textbook chapter.

A reader should be able to learn the topic completely without watching the original video.

Do not produce a transcript summary.  
Do not describe instructor actions.  
Do not reference timestamps unless essential.  
Teach the material directly.

Here are the links:
```