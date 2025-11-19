# PROMPT


---


## Task
Your task is to create a specification for an app in the @GEMINI.md file. Below, I will provide the technical details and requirements.

## Role
You are a well-versed specification writer for apps. Your specification should always help the AI generate neat and clean code, meaningful comments, and a professional industrial-practice folder structure.

## Function Requirements
- The function must verify that the email contains exactly one "@" symbol.
- The function must check for a valid domain, such as `.com` or `.org`.
- The function must return clear, descriptive error messages for any invalid input.
- The function should handle edge cases gracefully, such as empty input or missing domain.

## Tech Stack
- Frontend: **Next.js**  (directory: `app/`), **Tailwind CSS**, **Shadcn**
- Backend: **FastAPI**
- Database: Simple JSON DB in `src/` mvc architecture for MVP


---


---
# Result  OUTCOME:
---








# Gemini Specification: Email Validator Web App

## 1. Project Overview

This document outlines the specification for a web application that validates email addresses. The application will feature a clean, user-friendly frontend built with Next.js and a robust backend powered by FastAPI. The primary goal is to provide users with a simple interface to check if an email address is syntactically correct and to receive clear feedback on validation results.

## 2. Core Functionality: Email Validation Logic

The core of the application is a function that validates an email string based on the following rules. This logic will reside in the backend.

- **Presence of "@" Symbol:** The email must contain exactly one `@` symbol.
    - **Error:** "Invalid email: Must contain exactly one '@' symbol."
- **Local Part and Domain Part:** The `@` symbol must have non-empty text before (local part) and after it (domain part).
    - **Error (Local):** "Invalid email: Missing local part before '@'."
    - **Error (Domain):** "Invalid email: Missing domain part after '@'."
- **Valid Domain TLD:** The domain part must contain at least one dot (`.`) and have a valid Top-Level Domain (TLD). The TLD must be at least two characters long.
    - **Error (No dot):** "Invalid email: Domain must contain a '.'."
    - **Error (Invalid TLD):** "Invalid email: The domain's Top-Level Domain (e.g., .com) must be at least two characters long."
- **Edge Cases:**
    - **Empty Input:** The input string cannot be empty.
        - **Error:** "Input cannot be empty."
- **Successful Validation:** If all checks pass, the function should return a success message.
    - **Success:** "Email is valid."

## 3. Architectural Design

The application will follow a client-server architecture with a clear separation of concerns between the frontend and backend.

- **Frontend:** A Next.js application responsible for UI, user interaction, and communication with the backend.
- **Backend:** A FastAPI application providing a RESTful API for email validation logic.
- **Database:** A simple file-based JSON database for storing validation history (for MVP purposes).
- **Communication:** The frontend will make asynchronous HTTP requests (POST) to the backend API.

## 4. Backend (FastAPI)

The backend will be built using FastAPI and follow a Model-View-Controller (MVC) pattern within the `src/` directory.

### 4.1. Folder Structure

```
/
├── src/
│   ├── main.py             # FastAPI app instance and router
│   ├── controllers/
│   │   └── validation.py   # API endpoint logic
│   ├── models/
│   │   └── email.py        # Email validation business logic and Pydantic models
│   └── database/
│       └── history.json    # Simple JSON database
└── requirements.txt        # Python dependencies (fastapi, uvicorn, pydantic)
```

### 4.2. API Endpoint

- **Endpoint:** `POST /api/v1/validate`
- **Request Body (Pydantic Model in `src/models/email.py`):**
    ```python
    from pydantic import BaseModel

    class EmailRequest(BaseModel):
        email: str
    ```
- **Response Body (Pydantic Model in `src/models/email.py`):**
    ```python
    from pydantic import BaseModel

    class ValidationResponse(BaseModel):
        is_valid: bool
        message: str
        email: str
    ```
- **Logic (`src/controllers/validation.py`):**
    - The endpoint will receive the `EmailRequest`.
    - It will call the validation function from `src/models/email.py`.
    - It will log the validation attempt to `src/database/history.json`.
    - It will return a `ValidationResponse`.

### 4.3. Business Logic (`src/models/email.py`)

This file will contain the core `validate_email` function implementing the rules from Section 2.

## 5. Frontend (Next.js)

The frontend will be a single-page application built with Next.js, styled with Tailwind CSS, and utilizing Shadcn for UI components.

### 5.1. Folder Structure

```
/
├── app/
│   ├── layout.tsx              # Root layout
│   ├── page.tsx                # Main page component
│   └── components/
│       ├── email-validator.tsx   # Main interactive component
│       └── ui/                   # Shadcn UI components
├── lib/
│   └── utils.ts                # Utility functions (e.g., cn for Tailwind)
└── public/
```

### 5.2. UI Components

- **`email-validator.tsx`:**
    - This will be a client component (`'use client'`).
    - It will contain an `Input` field (from Shadcn) for the email and a `Button` to trigger validation.
    - It will use a state hook (e.g., `useState`) to manage the input value, loading state, and the validation result.
    - Upon button click, it will call the FastAPI backend.
    - It will display the validation result (`message`) and conditionally style the input or a result message (e.g., green for valid, red for invalid) using Tailwind CSS.
    - A `Card` component from Shadcn will wrap the entire UI for a clean look.

### 5.3. Styling and UI

- **Tailwind CSS:** Used for all custom styling.
- **Shadcn:** `Card`, `Input`, `Button`, and `Label` components will be used to build the primary UI. The theme and styling should be clean and professional.

## 6. Development Guidelines

- **Code Style:** Adhere to standard practices for Python (PEP 8) and TypeScript/React.
- **Comments:** Add meaningful comments to explain *why* something is done, especially for complex logic in the validation function. Do not comment on *what* is being done.
- **Folder Structure:** Strictly follow the defined folder structures for both frontend and backend to maintain a clean and scalable codebase.
- **Commits:** Use conventional commit messages (e.g., `feat:`, `fix:`, `docs:`).

