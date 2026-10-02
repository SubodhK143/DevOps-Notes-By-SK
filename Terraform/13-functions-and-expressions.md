# 🚀 Terraform Functions & Expressions

> Learn how **Terraform Functions** transform data and how **Expressions** make your configurations more dynamic and reusable.

---

# 📚 Table of Contents

- [What are Functions?](#-what-are-functions)
- [Why Use Functions?](#-why-use-functions)
- [Common Functions](#-common-functions)
- [What are Expressions?](#-what-are-expressions)
- [Conditional Expressions](#-conditional-expressions)
- [For Expressions](#-for-expressions)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 🧩 What are Functions?

Functions are built-in operations that help manipulate and transform values in Terraform.

They can work with:

- Strings
- Numbers
- Lists
- Maps
- Dates

> 💡 Functions make Terraform configurations more flexible and reduce repetitive code.

---

## 🤔 Why Use Functions?

Functions help you:

- Format values
- Join or split strings
- Convert data types
- Calculate values
- Simplify configurations

Example:

```hcl
upper("terraform")
```

Output:

```text
TERRAFORM
```

---

## ⚙️ Common Functions

| Function | Example | Result |
|----------|---------|--------|
| `upper()` | `upper("devops")` | `DEVOPS` |
| `lower()` | `lower("AWS")` | `aws` |
| `length()` | `length(["a","b"])` | `2` |
| `join()` | `join("-",["dev","01"])` | `dev-01` |
| `split()` | `split("-", "dev-01")` | `["dev","01"]` |
| `toset()` | `toset(["dev","prod"])` | Set of values |

---

## 📝 What are Expressions?

Expressions are used to calculate or reference values in Terraform.

Example:

```hcl
instance_type = var.instance_type
```

Terraform evaluates the expression and uses the resulting value.

---

## 🔀 Conditional Expressions

Conditional expressions choose a value based on a condition.

Syntax:

```hcl
condition ? true_value : false_value
```

Example:

```hcl
instance_type = var.environment == "prod" ? "t3.medium" : "t2.micro"
```

If the environment is **prod**, Terraform uses `t3.medium`; otherwise, it uses `t2.micro`.

---

## 🔁 For Expressions

For expressions transform collections such as lists or maps.

Example:

```hcl
[for env in ["dev", "prod"] : upper(env)]
```

Output:

```text
["DEV", "PROD"]
```

---

## ⭐ Best Practices

- Use functions to avoid repetitive code.
- Keep expressions simple and readable.
- Use conditional expressions for small decisions.
- Avoid overly complex nested expressions.
- Choose meaningful variable names.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Functions are
- ✅ Why Functions are useful
- ✅ Common Terraform functions
- ✅ What Expressions are
- ✅ Conditional expressions
- ✅ For expressions
- ✅ Best practices

---

## 📖 Next Topic

➡️ **14 - Terraform Provisioners**

In the next chapter, you'll learn:

- What are Provisioners?
- Types of Provisioners
- `local-exec`
- `remote-exec`
- When to use Provisioners
- Best practices
