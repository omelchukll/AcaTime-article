# Using AI to Write Scripts

AcaTime integrates with modern AI models to help you write effective optimisation scripts. Describe the rule in plain language and the system will generate the corresponding C# script automatically.

## AI Model and Requirements

The system currently uses the free **Gemini 2.0 Flash** model from Google.

To use the AI feature you need:
1. A Google account
2. A Google AI Studio API key
3. Enter the key in the AI assistant panel

## Getting a Google Gemini API Key

### Step 1 — Google Account

If you do not have one yet, go to [accounts.google.com](https://accounts.google.com) and create an account.

### Step 2 — Google AI Studio

1. Visit [aistudio.google.com](https://aistudio.google.com)
2. Sign in with your Google account
3. Accept the terms of use

### Step 3 — Create an API Key

1. Click **Get API key** (usually in the top-right corner)
2. Select **Create API key**
3. Enter a name (e.g. "AcaTime Integration")
4. Accept the terms → click **Create**
5. Copy the key (it looks like `AIzaSyA9lfQ6g_ViI7GbfE5uJIxQKQ9gLCSw7wI`)

---

## Generating a Script with AI

### Step 1 — Select the Script Type

1. Go to **Constraints & Scripts**
2. Click **Add New Script** or choose **Create with AI** for an existing script
3. Select the script type:
   - Schedule Evaluator
   - Day/Period Estimator
   - Day/Period Validator
   - Lesson Priority Estimator
   - Slot Constraint

### Step 2 — Describe the Rule

In the text field describe the rule in Ukrainian or English. Be as specific as possible:
- What behaviour do you want to achieve or prevent?
- What conditions must hold?
- What scoring criteria should be used?

**Good examples:**

> "Ban laboratory classes on Fridays after period 2 for 1st-year students"

> "If a teacher has two or more lectures on the same day, apply a penalty of −100"

**Bad examples:**

> "Make the schedule better"

> "Add a rule for teachers"

### Step 3 — Review and Save

After clicking **Generate**:
1. Review the generated code
2. Edit if needed
3. Validate (the system checks syntax and semantics automatically)
4. Save for use in schedule generation

---

## Free Model Limitations

1. **Request limits** — Google caps requests per minute and per day; intensive use may trigger temporary throttling
2. **Code quality** — the generated code may need additional editing for complex scenarios
3. **Response length** — very complex rules may need to be split into several scripts

---

## Further Reading

- [Script Types](./script-types.md)
- [Data Models](./models.md)
- [Algorithm Overview](./index.md)
