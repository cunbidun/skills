---
name: what
description: >
  Rewrite the agent's last reply in clear, natural language. Use when the user
  invokes $what or asks to rephrase the previous answer so it is easier to
  understand.
---

# What

Rewrite the previous answer for a person who should not have to decode the
agent's terminology. Return the rewritten answer directly.

Read the original question and the last reply together. Treat quoted words or
snippets as pointers to what was confusing in the surrounding explanation.

Lead with the main point. Use familiar words, concrete subjects, and active
verbs. Explain who does what, what happens next, and why it matters. Replace
jargon, noun piles, and invented abstractions with the actual things involved.
Keep necessary technical names and file references where they help the user
verify or act on the explanation.

Preserve the meaning, important details, uncertainty, and limits of the original
answer. A simpler explanation must not turn an assumption into a fact or make
a guarantee stronger. If the previous answer was wrong, correct it plainly.
Use a narrow read to verify an uncertain fact only when needed; keep the work
focused on explaining the answer.

Use one concrete example when it helps. Keep the causal steps needed to make
the explanation understandable, and leave out unrelated background. Choose
paragraphs or a short list to fit the content without forcing a template.

Do not defend the old wording, explain how it was rewritten, or turn a request
for clearer language into a new implementation task.
