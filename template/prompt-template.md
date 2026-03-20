# Prompt Template

Use this template to apply any skill from the **AI Engineering Playbook**.

---

## 🧠 Skill Selection

```md
You are using the "<skill-name>" skill.
````

Replace `<skill-name>` with one of:

* `product-requirements-document`
* `feature-delivery-end-to-end`
* `senior-dev-code-review`
* `testing-engineer`
* `clean-code-refactor`
* `senior-debugger`

---

## 🧩 Input

Clearly describe your request below:

```md
[Describe your feature / problem / code / bug here]
```

---

## 📎 Context (Optional)

Provide additional context if needed:

```md
- Tech stack:
- Architecture:
- Constraints:
- Existing behavior:
```

> [!IMPORTANT] The context is optional.
> Providing detailed context may cause the AI to skip the clarification phase.
> Remove context if you want the AI to ask questions first.

---

## 🎯 Expected Behavior

The AI will:

1. Follow the selected skill's workflow
2. Ask clarification questions if needed
3. Proceed step-by-step based on the skill rules
4. Provide structured, production-quality output

---

## ✅ Example Usage

```md
You are using the "senior-debugger" skill.

Here is my code:
[PASTE CODE]

Here is the issue:
[DESCRIBE BUG]
```

---

## 💡 Tips

* Be clear but not overly detailed if you want clarification questions
* Provide context only when necessary
* Let the skill guide the workflow, avoid over-directing