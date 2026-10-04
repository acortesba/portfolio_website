# 06-Security

## Contact Form Security Measures

### Character Limits Enforcement
To prevent oversized payloads and abuse, character limits have been implemented at both the HTML and JavaScript levels. A live character counter is visible for all input fields and textareas.

- **Name:** Maximum 100 characters.
- **Email:** Maximum 254 characters (RFC standard).
- **Company:** Maximum 100 characters.
- **Dynamic Role Questions (Textareas):** Maximum 500 characters.

The character counter provides real-time feedback. The `maxlength` HTML attribute natively prevents the user from typing beyond the limit, avoiding late validation errors and ensuring a smoother user experience.

### XSS Sanitization
The contact form inputs are processed and stored in Firebase and sent via EmailJS. We ensure that inputs are handled securely to prevent Cross-Site Scripting (XSS).
- The text content is extracted and validated without executing it as HTML.
- Firebase handles string sanitization when storing documents.
- EmailJS templates interpolate fields safely as text.
