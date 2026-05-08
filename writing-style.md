---
name: exam-answer-writing-style
description: Use when writing detailed college exam preparation answers from a chapter syllabus and question bank, especially for Full Stack Development, Spring Boot, backend, security, authentication, testing, or similar technical subjects.
---

# Exam Answer Writing Style

## Purpose

Use this style when creating reusable study notes or answer files from a question bank and syllabus. The output should help a college student prepare for theory-based and explanation-based exam questions.

## Required Inputs

Before writing, read:

- The chapter or unit syllabus.
- The complete question bank.
- Any user-provided subject context, such as course name, framework, technology stack, or exam purpose.

Use the syllabus to keep answers within the expected chapter scope. Do not add unrelated advanced topics unless they help explain the answer clearly.

## Output Format

Create a new Markdown answer file unless the user asks to edit an existing one.

Use this structure:

```markdown
# Unit/Chapter Title Answers

Subject: <subject name if known>
Unit: <unit or chapter name if known>

---

## 1. <Question text>

<Detailed answer>

---

## 2. <Question text>

<Detailed answer>
```

Number every answer exactly according to the question bank. Preserve duplicate questions if the question bank contains them, but answer them independently or with slightly varied wording.

## Answer Style

Write in a clear, exam-oriented style:

- Use simple technical language suitable for a college exam.
- Start with a direct definition or main idea.
- Explain the concept step by step.
- Connect the answer to the syllabus and subject context.
- For Spring Boot topics, mention practical components such as Spring Security, JWT filters, BCrypt, JUnit, Mockito, MockMvc, `@ControllerAdvice`, SLF4J, Logback, caching, profiling, or monitoring where relevant.
- Include examples when they make the answer easier to remember.
- Keep answers detailed enough for medium or long-form exam questions.
- Avoid overly short answers unless the question is very basic.
- Avoid unnecessary industry-level depth that is outside the syllabus.

## Handling Question Types

For theory questions:

- Define the term.
- Explain its purpose.
- List important points.
- Give a small example if useful.
- End with significance or benefit.

For flow/process questions:

- Explain the sequence in order.
- Use numbered steps or an ASCII flow diagram.
- Mention important decisions, status codes, or components.

For scenario/high-difficulty questions:

- Identify the concept involved.
- Explain what is happening in the scenario.
- State the likely cause or misconfiguration.
- Explain the correct behavior.
- Mention possible fixes or best practices.

## ASCII Diagrams

Use ASCII diagrams or flow charts whenever they improve understanding. Prefer simple diagrams over complex ones.

Examples:

```text
Client request
      |
      v
Spring Security Filter Chain
      |
      v
JWT validation
      |
      v
Allow request or reject
```

```text
Is user authenticated?
      |
   +--+--+
   |     |
  Yes    No
   |     |
Check  Return 401
role
```

Use ASCII diagrams for authentication flow, authorization flow, JWT structure, testing layers, exception handling, caching, monitoring, or any process-heavy answer.

## Code and Examples

Use short code snippets only when helpful. Prefer small, readable examples over full applications.

Examples of useful snippets:

```java
@PreAuthorize("hasRole('ADMIN')")
```

```java
@Cacheable("products")
```

```java
when(repository.findById(1L)).thenReturn(Optional.of(entity));
```

Use inline code formatting for framework names, annotations, HTTP status codes, headers, and important terms.

Examples:

- `Authorization: Bearer <token>`
- `401 Unauthorized`
- `403 Forbidden`
- `@ControllerAdvice`
- `BCryptPasswordEncoder`

## Tone

The tone should be:

- Clear
- Direct
- Student-friendly
- Technically correct
- Exam-focused

Do not write like marketing copy. Do not use jokes, emojis, or casual filler.

## Quality Checklist

Before finishing, verify:

- Every question from the question bank has an answer.
- The numbering matches the question bank.
- The answers follow the syllabus scope.
- The file is readable as standalone exam preparation material.
- ASCII diagrams are valid plain text and not visually broken.
- Technical terms are explained before being used heavily.
- Scenario answers include cause, explanation, and correct behavior.

