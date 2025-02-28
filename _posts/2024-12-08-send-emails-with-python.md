---
title: 'Send Emails with Python: Customizing Sender Email'
date: 2024-12-08
permalink: /posts/2024/12/send-emails-with-python/
tags:
  - Python
  - sending email
  - automation
  - customize email header
---

In this blog post, we will explore how to send emails using Python, with a special focus on customizing the "From" header to display a different email address or name, while the actual sender email address remains unchanged. This is useful when you want to send an email from one address but have the `From` field show a different email or name, creating a more personalized or professional appearance.

**Prerequisites**
Before we dive into the code, let's make sure you have the required tools:

- **Python 3.x** installed on your system. [**smtplib**](https://docs.python.org/3/library/smtplib.html) and [**email.mime**](https://docs.python.org/3/library/email.mime.html) libraries are included in the Python standard library, so you don’t need to install anything extra.
- Access to an SMTP server. For this example, we’ll use Gmail’s SMTP server, but you can modify the code to work with any SMTP provider.

**Why Would You Want to Customize the `From` Header?**
You might want to send an email from your address but show a different "From" name or email address for various reasons:

- **Marketing Emails**: You may want the email to appear as if it’s coming from a company or a support team, even though it’s sent from your personal email.
- **Multiple Email Aliases**: If you use aliases for different purposes (e.g., support@company.com but the email is sent from your_email@example.com), you can display the alias in the header.
- **Professional Appearance**: Customize the display name to appear more formal or specific to your communication, like "Customer Support" or "John Doe" instead of your actual email.

**Steps to Send an Email with a Different `From` Header**

**Step 1: Set Up the Email Message**

We will use Python’s built-in `smtplib` to send the email and `email.mime` to construct the email message. Let us first import the necessary libraries.

```
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
```

Then define the sender email, receiver email, and SMTP server details as follows: 

```
sender_email = "your_email@example.com"  # Email that will send the email
receiver_email = "receiver_email@example.com"
password = "your_email_password" # TIP: security consideration
```

Next, set up the SMTP server (Gmail is used as an example)

```
smtp_server = "smtp.gmail.com"
smtp_port = 587
```
**Step 2: Create the email message and customize the `From` field**
Now let us create the email message and customize it. Here is where you can customize the `From` field.

```
msg = MIMEMultipart()

msg['From'] = "Custom Name <your_email@example.com>"  # The 'From' field will show this name
msg['To'] = receiver_email
msg['Subject'] = "Test Email from Python"

# Email body content
body = "Hello, this is a test email sent from Python!"

# Attach the email body to the message
msg.attach(MIMEText(body, 'plain'))
```

In the `msg['From']` field, you can change both the name and the email address. By default, the From header will show the email address. If you make it show a different name or alias, the recipient will see "Custom Name" in their inbox, but the email will still be sent from your_email@example.com.

**Step 3: Log In to the SMTP Server and Send**

Now that we have set up the email message, let's log in to the SMTP server and send the email:

```
try:
    # Set up the SMTP server connection
    server = smtplib.SMTP(smtp_server, smtp_port)
    server.starttls()  # Start TLS encryption for secure email sending

    # Log in to the SMTP server with your credentials
    server.login(sender_email, password)

    # Send the email
    server.sendmail(sender_email, receiver_email, msg.as_string())
    print("Email sent successfully!")

except Exception as e:
    print(f"Failed to send email: {e}")

finally:
    # Close the server connection
    server.quit()
```

In this example, we used `smtplib.SMTP` to connect to the SMTP server. For Gmail, the server is `smtp.gmail.com` and the port is `587`. We also use `starttls()` to encrypt the connection, ensuring that our credentials and email content are secure. 

The `server.login()` function logs into the server using your sender email and password. If you’re using Gmail and have two-factor authentication (2FA) enabled, you’ll need to generate an ***App Password*** instead of using your regular Gmail password. This is a security measure to prevent unauthorized access. Follow the [instruction on how to create App Password](https://myaccount.google.com/apppasswords) to get one. 

The try block ensures that any issues during the email sending process are caught, and the exception is displayed if there’s a problem.

**Security Considerations**

Never hard-code your email password in your scripts. Consider using environment variables or a credentials manager to securely store your login credentials.
If you're using Gmail, remember to either enable Less Secure Apps or generate an App Password if two-factor authentication is enabled.

**Conclusion**
Sending emails programmatically with Python is a powerful feature, especially when you need to customize the header or automate the email sending process, e.g., if you are sending marketing emails, notifications, or personal messages, this approach provides you with the flexibility to display any name or email address in the header, even if the actual sender email is different.