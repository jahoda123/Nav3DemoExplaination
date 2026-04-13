# 📱 Jetpack Compose Navigation 3 – Cheatsheet

This document explains your code **step by step**, including *what it does* and *why it is used*.

---

# 🧠 Overview

This app uses **Navigation 3 (Nav3)** with Jetpack Compose.

It has:

* A **Home screen**
* A **Profile screen**
* A **navigation back stack**

Navigation works by pushing and popping objects (`NavKey`) onto a stack.

---

# 🧩 Core Concepts

## 1. NavKey (Your Routes)

```kotlin
@Serializable
data class Home(val title: String) : NavKey

@Serializable
data class Profile(val title: String) : NavKey
```

### ✅ What it does

* Represents a **screen (route)**
* Holds data (like `title`)

### 🤔 Why it's needed

* Navigation 3 is **type-safe** (no strings like "home_screen")
* You pass **real objects instead of routes**

---

## 2. BackStack (Navigation State)

```kotlin
val backStack = rememberNavBackStack(Home("Home"))
```

### ✅ What it does

* Stores navigation history
* First screen = `Home("Home")`

### 🤔 Why it's needed

* Controls which screen is visible
* Enables back navigation

---

## 3. NavDisplay (Navigation Engine)

```kotlin
NavDisplay(
    backStack = backStack,
    onBack = { backStack.removeLastOrNull() },
    entryProvider = entryProvider { ... }
)
```

### ✅ What it does

* Displays the **current screen** from the back stack

### 🤔 Why it's needed

* Replaces `NavHost` from older navigation
* Works with type-safe `NavKey`

---

## 4. entryProvider (Screen Mapping)

```kotlin
entry<Home> { key -> ... }
entry<Profile> { key -> ... }
```

### ✅ What it does

* Maps each `NavKey` → corresponding screen

### 🤔 Why it's needed

* Navigation must know **what UI to show** for each key

---

# 🏠 Home Screen Flow

```kotlin
HomeScreen(
    title = key.title,
    onProfileClick = {
        backStack.add(Profile("Profile"))
    }
)
```

### ✅ What happens

1. User is on Home
2. Clicks button
3. New screen is pushed:

```kotlin
backStack.add(Profile("Profile"))
```

### 🔥 Result

➡️ App navigates to Profile screen

---

# 👤 Profile Screen Flow

```kotlin
onBackClick = { backStack.removeLastOrNull() }
```

### ✅ What happens

* Removes top screen from stack

### 🔥 Result

➡️ Goes back to Home

---

# 🔁 Navigation Mechanism (IMPORTANT)

## Push (Navigate forward)

```kotlin
backStack.add(Profile(...))
```

➡️ Adds new screen

---

## Pop (Go back)

```kotlin
backStack.removeLastOrNull()
```

➡️ Removes current screen

---

# 🧱 UI Structure

## Scaffold

```kotlin
Scaffold { innerPadding -> ... }
```

### ✅ What it does

* Provides layout structure (safe areas, padding)

---

## Column Layout

```kotlin
Column(
    verticalArrangement = Arrangement.Center,
    horizontalAlignment = Alignment.CenterHorizontally
)
```

### ✅ What it does

* Centers UI vertically + horizontally

---

# ✏️ Input Handling

```kotlin
val inputState = rememberTextFieldState(initialText = "Hello")
```

### ✅ What it does

* Stores text input state

### 🤔 Why it's needed

* Keeps text during recomposition

---

## Passing Data Between Screens

```kotlin
onProfileClick(inputState.text.toString())
```

➡️ Value is passed to:

```kotlin
Profile(title = input)
```

### 🔥 Result

* Profile screen shows dynamic title

---

# 🧪 Preview Function

```kotlin
@Preview
```

### ✅ What it does

* Shows UI inside Android Studio

### ⚠️ Special detail

* Uses its own `backStack`

---

# ⚠️ Important Notes

## 1. Type-safe Navigation

* No string routes
* Safer and cleaner

## 2. Serializable Required

* Needed to save/restore navigation state

## 3. Back Handling

```kotlin
onBack = { backStack.removeLastOrNull() }
```

* Handles system back button

---

# 🧭 Full Flow Summary

1. App starts → `Home("Home")`
2. User enters text
3. Clicks button
4. Navigates to Profile with input
5. Clicks back
6. Returns to Home

---

# 💡 Why This Approach is Better

✅ Type-safe navigation

✅ No route strings

✅ Easier data passing

✅ Cleaner architecture

---

# 🚀 Mental Model

Think of navigation like a **stack of cards**:

```
[ Home ]

Click →

[ Home, Profile ]

Back →

[ Home ]
```

---

# 🎯 Key Takeaways

* `NavKey` = screen
* `BackStack` = navigation history
* `NavDisplay` = renders UI
* `entryProvider` = connects keys to screens
* `add()` = navigate forward
* `removeLastOrNull()` = go back

---
