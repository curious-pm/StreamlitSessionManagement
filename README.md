## Join the Community  
[Join Curious PM Community](https://curious.pm/) to connect, share, and learn with others!

---

A Streamlit-based tool to implement secure session management, ensuring user authentication, session persistence, and expiration handling.

# Session Management in Streamlit

## About This Project
The Streamlit Session Management Template is a simple web app built with Streamlit that allows developers to manage user sessions securely. It provides functionality to log in, maintain session persistence, and handle session expiration through encrypted cookies.

---

## What This App Does  

- **User Authentication:** Login system with secure session management.  
- **Session Persistence:** Retain user sessions across page reloads.  
- **Session Expiry:** Automatically logs out users after a set duration.  
- **Logout Option:** Allows users to log out anytime securely.  
- **Error Handling:** Provides feedback for incorrect credentials or session issues.  

### Key Features  

- **Simple Interface:** Easy-to-use login/logout system.  
- **Encrypted Cookies:** Secure storage of session data.  
- **Customizable Timeout:** Configurable session expiration time.  
- **User-Friendly Notifications:** Displays alerts for session status changes.  

---

## How to Use the App (Step-by-Step)  

### 1. Install Requirements  

Make sure you have Python installed, then install required libraries:  

```bash
pip install streamlit streamlit-cookies-manager
```

---

### 2. Set Up the Project  

Organize your files like this:  

```
Session Management/
├── README.md                 # Project documentation
├── app.py                     # Main Streamlit application script
└── requirements.txt              # List of dependencies required to run the project
```

---

### 3. Run the App  

Start the app with this command:  

```bash
streamlit run app/main.py
```

---

### 4. Use the App  

1. **Login:**  
   - Enter your username and password to authenticate.  
   - Upon successful login, the session is created.  

2. **Session Management:**  
   - The session persists until it expires or the user logs out.  

3. **Logout:**  
   - Click the logout button to terminate the session.  

4. **Session Expiry:**  
   - If inactive for a set period, the session expires automatically.  

---

### 5. Implementing It Yourself  


### 1. Install Required Libraries
Install Streamlit and the `streamlit-cookies-manager` library for managing encrypted cookies:

```bash
pip install streamlit streamlit-cookies-manager
```

---

### 2. Define Constants
Set configurable parameters for the session timeout duration, cookie keys, and user credentials.

```python
SESSION_TIMEOUT_MINUTES = 10  # Session validity duration in minutes
SESSION_COOKIE_KEY = "session_start_time"  # Key for session start time in cookies
USER_CREDENTIALS = {
    "admin": "password123",  # Default admin account
    "user1": "mypassword",   # Example user account
}  # Replace with secure storage for production
```

---

### 3. Initialize the Cookie Manager
Use the `EncryptedCookieManager` to securely store session data in cookies.

```python
cookies = EncryptedCookieManager(
    prefix="secure_session_",  # Prefix for cookie keys
    password="your_secure_password"  # Encryption password (replace for production)
)

# Ensure cookies are ready
if not cookies.ready():
    st.stop()
```

---

### 4. Implement Helper Functions

- **Session Validation**: Checks if the session is still valid based on the session start time stored in cookies.

```python
def is_session_valid():
    session_start_time = cookies.get(SESSION_COOKIE_KEY)
    if session_start_time:
        session_start_time = datetime.strptime(session_start_time, "%Y-%m-%d %H:%M:%S")
        if datetime.now() - session_start_time < timedelta(minutes=SESSION_TIMEOUT_MINUTES):
            return True
    return False
```

- **Refresh Session**: Updates the session start time to extend the session.

```python
def refresh_session():
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    if cookies.get(SESSION_COOKIE_KEY) != current_time:
        cookies[SESSION_COOKIE_KEY] = current_time
        cookies.save()
```

- **Clear Session**: Clears the session by setting the cookie value to an empty string.

```python
def clear_session():
    if cookies.get(SESSION_COOKIE_KEY):
        cookies[SESSION_COOKIE_KEY] = ""
        cookies.save()
```

---

### 5. Create the UI Functions

- **Login UI**: Displays a login form and validates user credentials.

```python
def login_ui():
    st.title("Login to Your Account")
    username = st.text_input("Username", placeholder="Enter your username")
    password = st.text_input("Password", type="password", placeholder="Enter your password")
    
    if st.button("Login"):
        if username in USER_CREDENTIALS and USER_CREDENTIALS[username] == password:
            refresh_session()
            st.success("Login successful! Refreshing page...")
            st.rerun()
        else:
            st.error("Invalid credentials. Please try again.")
```

- **Main App UI**: Displays the application content for logged-in users.

```python
def main_app_ui():
    st.title("Welcome to the Secure Streamlit App")
    st.markdown("This is a session-protected application.")
    st.write("You are currently logged in.")
    
    if st.button("Logout"):
        clear_session()
        st.rerun()
```

---

### 6. Add Main Logic

The application logic determines whether to display the login or main UI based on session validity:

```python
if not is_session_valid():
    clear_session()
    login_ui()
else:
    main_app_ui()
```

---

## Security Considerations

1. **Use Secure Storage:** Store credentials securely instead of hardcoding.
2. **Environment Variables:** Use environment variables to store sensitive information.
3. **Session Expiry:** Implement proper session expiration handling.
4. **Secure Cookies:** Enable HTTP-only and secure flags to prevent client-side access.

---

## Deployment

1. **Secure API Keys:** Store credentials securely using environment variables or `.streamlit/secrets.toml`.
2. **Customize the Interface:** Modify layout and styling using Streamlit widgets.
3. **Deploy the Application:** Use Heroku, AWS, or Streamlit Cloud to deploy your app.

---

## What Happens at Each Step  

### 1. User Logs In  
- Credentials are verified, and an encrypted session cookie is created.  
- Session start time is stored securely in cookies.  

### 2. Session Persistence  
- The app checks for an active session across page reloads.  
- If valid, the user remains logged in.  

### 3. Session Timeout  
- If the session exceeds the timeout limit, the user is logged out automatically.  

### 4. User Logs Out  
- Session data is cleared from cookies, and the login screen is shown.  

---
### Folder and File Descriptions

```
Session Management/
├── README.md                 # Project documentation
├── app.py                     # Main Streamlit application script
└── requirements.txt           # List of dependencies required to run the project
```



- **`README.md`** – Provides documentation about the project, including installation, usage, and troubleshooting.
- **`app.py`** – The main Streamlit application script handling user authentication and session management.
- **`requirements.txt`** – Contains the list of dependencies needed to run the application.


---

## Hosted App  

[View Live App](https://sessionmanagement-template.streamlit.app/) 

---

## Screenshots  

### Login Page:
![Image](https://github.com/user-attachments/assets/e6ce1636-4e36-4b34-83eb-328b34e07df1)

### Logged-In Session:
![Image](https://github.com/user-attachments/assets/b3baf3bf-b55e-4ce6-ad53-60978fb6a34c)

---

## Video Overview  
*A short video walkthrough will be provided explaining the app's features and usage.*  

---

## Security Considerations  

- **Do not share your API keys publicly.**  
- Use environment variables to securely store credentials.  
- Regularly rotate API keys for added security.  

---

## Common Issues & Solutions  

1. **Incorrect Credentials:**  
   - Double-check the username and password entered.  

2. **Session Not Persisting:**  
   - Ensure cookies are enabled in your browser settings.  

3. **App Not Running:**  
   - Run the correct command: `streamlit run app/main.py`.  

4. **Session Timeout Issues:**  
   - Adjust timeout settings in `session_utils.py`.  

---

## Security Tips  

- Never store credentials in source code.  
- Use HTTPS for secure deployment.  
- Regularly review session logs for unusual activities.  

---
