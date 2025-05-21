# NetlifyCMS Setup and Usage Guide

This guide explains how to complete the setup for NetlifyCMS and how to use it to manage content on your Eleventy website.

## 1. Initial Setup (One-time)

There are a couple of manual steps you need to perform in Netlify and in the codebase to get the CMS fully working.

### a. Configure Repository in `admin/config.yml`

The CMS needs to know which GitHub repository to connect to.

1.  Open the file `src/admin/config.yml`.
2.  Find the `backend` section:
    ```yaml
    backend:
      name: github
      repo: user/repo # <-- UPDATE THIS LINE
      branch: main
    ```
3.  **Important:** Change `repo: user/repo` to your actual GitHub repository details in the format `owner_name/repository_name`. For example, if your repository URL is `https://github.com/yourusername/your-awesome-blog`, you would set it to `repo: yourusername/your-awesome-blog`.
4.  Save the file and commit this change to your repository.

### b. Enable Netlify Identity

Netlify Identity is used to authenticate users for the CMS.

1.  Go to your site's dashboard on [Netlify](https://app.netlify.com/).
2.  Navigate to **Site settings > Identity**.
3.  Click **Enable Identity**.
4.  Once enabled, under **Registration preferences**, you can choose how you want to allow users to register. For a personal site, "Invite only" is a secure option.
5.  If you want to allow external providers (like Google or GitHub for login), you can enable them under **External providers**.
6.  Scroll down to **Services** and click **Enable Git Gateway**. This allows NetlifyCMS to interact with your GitHub repository on behalf of authenticated users. Ensure the roles are set correctly (you can usually leave this to default).

## 2. Accessing the CMS

Once Netlify Identity is enabled and you have invited users (or allowed open registration):

1.  Go to `[your-website-url]/admin/`. For example, if your site is `https://example.com`, the CMS is at `https://example.com/admin/`.
2.  You should see the Netlify Identity login prompt. Log in with your credentials.

## 3. Using the CMS

After logging in, you'll see the NetlifyCMS interface.

*   **Collections:** On the left sidebar, you'll see a list of "Collections" (e.g., "Blog"). These are the types of content you can manage.
*   **Creating New Content:**
    *   Click on a collection (e.g., "Blog").
    *   Click the "New [Collection Name]" button (e.g., "New Blog").
    *   Fill in the fields (Title, Date, Body, etc.).
    *   Click "Publish" (or "Save" if you want to keep it as a draft, though draft functionality might depend on your Eleventy setup).
*   **Editing Existing Content:**
    *   Click on a collection.
    *   Click on the piece of content you want to edit from the list.
    *   Make your changes.
    *   Click "Publish".
*   **Media:**
    *   When editing content, you can usually find a "Media" button or an "Insert Image" option in the Markdown editor to upload images.
    *   Uploaded images will be stored in the `src/assets/images/uploads` folder (as configured in `admin/config.yml`).

## Important Notes

*   **Deployment:** When you publish changes through NetlifyCMS, it creates a new commit in your GitHub repository. This will trigger a new build and deployment on Netlify automatically.
*   **Drafts:** How drafts are handled depends on your Eleventy configuration. By default, NetlifyCMS might save unpublished changes as pull requests or in a separate branch. Your current Eleventy setup includes a drafts plugin, which might control visibility of posts not yet published or with future dates.
*   **Troubleshooting:** If you encounter issues, check the browser's developer console for error messages. You can also refer to the [NetlifyCMS Documentation](https://www.netlifycms.org/docs/intro/).
