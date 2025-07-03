# Guide: Rolling Deployments (Blue-Green) with GitHub and Vercel

This guide demonstrates a "Blue-Green" rolling deployment strategy. This technique allows you to release new versions of your website with zero downtime and provides an instant "rollback" option if something goes wrong.

**The Concept:**
-   **Blue Version:** The current, stable website that your users see.
-   **Green Version:** A new, updated version of the site, ready to go live but currently hidden from the public.
-   **The "Roll Out":** Instantly switching the live traffic to point from the Blue Version to the Green Version.

---

### Part 1: Deploy the "Blue" Version (The Initial Setup)

1.  **Create a GitHub Repository:**
    *   Create a new, public repository on GitHub named `rolling-deployment-demo`.
    *   Initialize it with a `README.md` file.

2.  **Create the "Blue" Website File:**
    *   In the repository, create a new file named `index.html`.
    *   Paste the following "Blue Version" code into it and commit it to the `main` branch.
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Rolling Deployment Demo</title>
        <style>
            body { 
                font-family: sans-serif; 
                display: grid; 
                place-content: center; 
                height: 100vh; 
                margin: 0; 
                background-color: #e6f7ff; /* Light Blue Background */
            }
            h1 { color: #005a9c; } /* Dark Blue Text */
        </style>
    </head>
    <body>
        <h1>BLUE VERSION</h1>
        <p>This is the current live version of the website.</p>
    </body>
    </html>
    ```

3.  **Deploy to Vercel:**
    *   Go to your Vercel dashboard and import this new GitHub repository.
    *   You may need to adjust GitHub App permissions to allow Vercel to see the new repository.
    *   Click "Deploy" without changing any settings. Your `rolling-deployment-demo.vercel.app` URL is now your live **Blue Version**.

---

### Part 2: Prepare the "Green" Version (The Next Release)

1.  **Create a Pull Request on GitHub:**
    *   Go to your `index.html` file on GitHub and edit it.
    *   Replace the code with the "Green Version" code below.
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Rolling Deployment Demo</title>
        <style>
            body { 
                font-family: sans-serif; 
                display: grid; 
                place-content: center; 
                height: 100vh; 
                margin: 0; 
                background-color: #e6fff2; /* Light Green Background */
            }
            h1 { color: #006400; } /* Dark Green Text */
        </style>
    </head>
    <body>
        <h1>GREEN VERSION</h1>
        <p>This is the new version, ready to go live!</p>
    </body>
    </html>
    ```
    *   **CRUCIAL:** When committing, select **"Create a new branch for this commit and start a pull request."** This keeps your new code separate from the live `main` branch.
    *   Create the Pull Request. Vercel will automatically build a preview. This preview is your **Green Version**.

---

### Part 3: The Roll Out and The Rollback

This is where we manage the live traffic from the Vercel dashboard.

#### A) Promote Green to Production (The "Roll Out")

1.  **Do NOT merge the Pull Request yet.**
2.  Go to your Vercel project dashboard and open the **Deployments** tab.
3.  You will see your deployments. The one from your new branch (e.g., `patch-1`) is your **Green Version**.
4.  On the row for the Green Version, click the **three dots (⋮)** and select **"Promote to Production"**.
5.  Confirm the action.
6.  **Verify:** Refresh your live website URL. It is now green!

#### B) Revert to Blue (The "Instant Rollback")

Imagine the green version has a bug! You need to go back to the stable blue version immediately.

1.  Go back to the **Deployments** tab on Vercel.
2.  Find the **original** deployment from the `main` branch (it will be near the bottom). This is your old **Blue Version**.
3.  On the row for the Blue Version, click the **three dots (⋮)** and select **"Promote to Production"**.
4.  **Verify:** Refresh your live website URL. It is instantly blue again!

---

### Part 4: Final Cleanup on GitHub

After you have promoted a version and are happy with it, you should update your `main` branch to match.

1.  Go back to your open Pull Request on GitHub.
2.  Click **"Merge pull request"** and confirm.
3.  Click **"Delete branch"** to keep your repository tidy.

**Congratulations!** You have successfully performed a Blue-Green deployment and an instant rollback—two of the most powerful techniques for safe and modern web development.
