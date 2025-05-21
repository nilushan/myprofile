# NetlifyCMS with GitHub Authentication Setup Guide

This guide explains how to set up NetlifyCMS to use GitHub authentication for managing content on your Eleventy website deployed via GitHub Pages.

## 1. Initial Configuration (Codebase)

Before setting up GitHub authentication, ensure your CMS configuration file is correctly set up.

### a. Configure Repository and Site Domain in `src/admin/config.yml`

The CMS needs to know which GitHub repository to connect to and your site's live URL.

1.  Open the file `src/admin/config.yml`.
2.  Locate the `backend` section. It should look similar to this:
    ```yaml
    backend:
      name: github
      repo: user/repo # <-- UPDATE THIS LINE with your_username/your_reponame
      # branch: main # Optional: uncomment and set if your default branch isn't 'main' or 'master'
      base_url: https://api.github.com
      auth_endpoint: auth
      site_domain: your-github-username.github.io/your-repo-name # <-- UPDATE THIS LINE
    ```
3.  **Important:**
    *   Change `repo: user/repo` to your actual GitHub repository details in the format `owner_name/repository_name`. For example, if your repository URL is `https://github.com/yourusername/your-awesome-blog`, set it to `repo: yourusername/your-awesome-blog`.
    *   Change `site_domain: your-github-username.github.io/your-repo-name` to your actual live GitHub Pages site URL. For example, `https://yourusername.github.io/your-awesome-blog`.
4.  Save the file and commit this change to your repository if you haven't already.

## 2. Create a GitHub OAuth Application

NetlifyCMS requires a GitHub OAuth application to authenticate users.

1.  Go to your GitHub **Developer settings**:
    *   Click on your profile picture in the top-right corner.
    *   Select **Settings**.
    *   In the left sidebar, scroll down and click **Developer settings**.
    *   Click on **OAuth Apps** in the left sidebar.
2.  Click the **New OAuth App** button.
3.  Fill in the application details:
    *   **Application name:** Choose a descriptive name, e.g., `My Site CMS` or `[Your Repo Name] CMS`.
    *   **Homepage URL:** Enter the URL of your live GitHub Pages site (e.g., `https://yourusername.github.io/your-awesome-blog`). This should match the `site_domain` you set in `config.yml`.
    *   **Application description (Optional):** A brief description.
    *   **Authorization callback URL:** This is crucial. It should be your **Homepage URL** followed by `/admin/`. For example: `https://yourusername.github.io/your-awesome-blog/admin/`.
        *   *Note:* The `/admin/` path is where your `admin/index.html` (the CMS entry point) will be served.
4.  Click **Register application**.
5.  You will be taken to the OAuth App's page. **Keep this page open.** You will need the **Client ID** from here.
    *   You can also generate a **Client Secret** if needed, but for public clients like a CMS running in the browser, only the Client ID is typically used by NetlifyCMS for the implicit grant flow. NetlifyCMS handles this automatically.

## 3. Accessing and Using the CMS

1.  Ensure all your changes (to `config.yml`, `admin/index.html`, etc.) are pushed to your GitHub repository and your GitHub Pages site is updated.
2.  Go to the **Authorization callback URL** you configured in the GitHub OAuth app. This is your CMS admin page, e.g., `https://yourusername.github.io/your-awesome-blog/admin/`.
3.  You should be redirected to GitHub to authorize the OAuth application. Click **Authorize [Your Application Name]**.
4.  After authorization, you will be redirected back to the CMS interface.
5.  You can now use the CMS to manage your content:
    *   **Collections:** On the left sidebar, you'll see "Blog" or other collections you've defined.
    *   **Creating/Editing:** Select a collection, then either create a new entry or edit an existing one.
    *   **Publishing:** When you save or publish changes, NetlifyCMS will commit them directly to your configured GitHub repository branch. This will then trigger a new GitHub Pages build.

## Important Notes

*   **Repository Permissions:** The GitHub user authenticating via OAuth must have write permissions to the repository specified in `config.yml`.
*   **GitHub Pages Build Time:** After NetlifyCMS pushes a commit, allow some time for GitHub Pages to rebuild and deploy your site.
*   **Troubleshooting:**
    *   Double-check all URLs in `config.yml` and your GitHub OAuth App settings (especially the Authorization callback URL).
    *   Clear your browser cache and cookies if you encounter login loops.
    *   Check the browser's developer console for error messages.
    *   Refer to the [NetlifyCMS Documentation on GitHub backend](https://www.netlifycms.org/docs/github-backend/).
