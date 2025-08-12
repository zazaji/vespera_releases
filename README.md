
<!-- README.md -->
<div align="center">
  <img src="https://raw.githubusercontent.com/zazaji/vespera/main/src-tauri/icons/128x128@2x.png" alt="Vespera Logo" width="128">
  <h1>Vespera</h1>
  <p><strong>Your private, encrypted, cloud-native digital sanctum.</strong></p>
  <p>
    <a href="./README_zh.md">中文</a> | English
  </p>
</div>

[![Release](https://github.com/zazaji/vespera/actions/workflows/release.yml/badge.svg)](https://github.com/zazaji/vespera/actions/workflows/release.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Vespera** is a serverless,and end-to-end encrypted journal for your most personal thoughts. With a focus on absolute privacy, we do not provide servers; you are required to set up your own database and object storage. Your notes are never stored locally and are only read temporarily from the database. They are sealed with zero-knowledge encryption before they ever leave your device. Data service providers and others cannot read them. No one can.

This is not just a notes app; it's your digital sanctum, built upon an infrastructure that is **entirely under your control**.

---

### 💡 Core Philosophy

Vespera is built on several core principles:

*   🔐 **Absolute Privacy**: Your data is end-to-end encrypted (E2EE) with a key only you know. All encryption and decryption happen locally on your device.
*   ☁️ **You Own Your Data**: Vespera has no servers. It connects directly to **your personal**, free-tier-friendly cloud infrastructure, such as Cloudflare R2 (for S3-compatible storage) and Supabase (for a PostgreSQL database). You have complete control.
*   🚀 **Modern & Fast**: Built with Rust, Tauri, Vue 3, and TypeScript to provide a native, fast, and responsive experience for Windows, macOS, and Linux users.
*   👐 **Open & Transparent**: The client is fully open-source. You can audit the code, build it yourself, and be confident in the software you're running.

### ✨ Features

*   **Multi-Account Support**: Manage separate, isolated sets of notes (e.g., personal, work).
*   **Powerful Markdown Editor**: Featuring live preview, split-screen mode, and a rich formatting toolbar. LaTeX support is included.
*   **File & Folder Organization**: A familiar tree structure with drag-and-drop support.
*   **Advanced Tagging System**: Organize notes with tags and filter by them.
*   **Color Coding**: Assign colors to notes and folders for visual management.
*   **Limited Search**: Instantly find notes with a powerful search engine that supports CJK languages. We stipulate that the first line of each file is not encrypted and is stored in the database, allowing keyword searches on this line only. You can configure keywords on the first line of each file for convenient searching.
*   **Image Uploads**: Simply paste images into the editor, and they are automatically uploaded to your S3 bucket.
*   **Enhanced Security**:
    *   **Account Lock**: Protect the entire application with a passcode.
    *   **Folder Lock**: Encrypt specific folders with a separate password.
    *   **Auto-Lock**: Automatically lock the app after a period of inactivity.
*   **Data Portability**: Easily export all your notes to a JSON file or import from a backup.
*   **Theming**: Light, Dark, Nord, and Solarized themes to fit your style.
*   **Drawback**: As there is no local storage and all data is loaded temporarily from object storage, the efficiency might not match other note-taking software.

### 🏗️ How It Works: An Architecture of Privacy

Vespera's security model is designed to give you peace of mind.

1.  **Setup**: You provide your credentials for your own PostgreSQL database (from Supabase) and S3-compatible object storage (from Cloudflare). You also set a unique **Encryption Key**.
2.  **Local Storage**: Your account configurations are stored encrypted on your local machine. Your master encryption key is never stored in an unencrypted form.
3.  **Data Storage**:
    *   **Note Content**: When you write a note, the content is encrypted on your device using your encryption key and then uploaded to your S3 bucket.
    *   **Metadata**: The file structure, tags, and note titles are stored in your PostgreSQL database. This allows for fast searching and rendering of the directory tree without needing to download and decrypt every single note.
4.  **Syncing**: When you use Vespera on another device, you can export an encrypted account string and copy it to the new device (requiring you to set an AES key). It will follow the same process, connecting to your cloud services and decrypting the data locally.

### 🚀 Getting Started

To use Vespera, you'll need to set up your own free cloud backend. This takes about 10 minutes.

#### 1. Set up Supabase (PostgreSQL Database)

1.  Go to [Supabase](https://supabase.com/) and create a new project.
2.  Once your project is ready, navigate to **Project Settings** > **Database**.
3.  Under **Connection string**, find the `URI` which is formatted as `postgresql://postgres:[YOUR-PASSWORD]@[HOST]:[PORT]/postgres`. Choose the string with port 5432, not 6543. (I was stuck here for 3 days with no progress).
4.  **This is your Database URL.** Copy it.

#### 2. Set up Cloudflare R2 (S3-Compatible Storage)

1.  Go to your [Cloudflare Dashboard](https://dash.cloudflare.com/).
2.  Navigate to **R2** > **Create bucket**. Give it a name (e.g., `vespera-notes`).
3.  From the R2 overview page, click **Manage R2 API Tokens** on the right.
4.  Click **Create API Token**.
5.  Give the token a name, select **Object Read & Write** for permissions, and click **Create API Token**.
6.  The page will display your **Access Key ID** and **Secret Access Key**. **Copy these immediately**, as the secret key will only be shown once.
7.  Go back to your R2 bucket page. The **S3 Endpoint** will be listed there. It looks like `https://[ACCOUNT_ID].r2.cloudflarestorage.com`.

#### 3. Configure Vespera

1.  Download and open the latest release of Vespera.
2.  On the setup screen, enter the credentials you just collected:
    *   **Account Name**: A name for this account (e.g., "My Personal Notes").
    *   **Database URL**: Your Supabase connection string.
    *   **S3 Endpoint**: Your Cloudflare R2 endpoint.
    *   **S3 Region**: Enter `auto`.
    *   **S3 Bucket Name**: The name of the bucket you created.
    *   **S3 Access Key ID / Secret Access Key**: The API token credentials from Cloudflare.
    *   **Encryption Key**: **Create a strong and unique password.** This is the master key for your notes. **If you lose it, your data is unrecoverable.**
3.  Test the connections, save, and you're ready to go!

### 🛠️ Building from Source

If you'd like to build Vespera yourself:

1.  **Prerequisites**:
    *   Node.js and Yarn/npm
    *   Rust and Cargo
    *   [Tauri prerequisites](https://tauri.app/v1/guides/getting-started/prerequisites) for your OS.

2.  **Clone the repo**:
    ```sh
    git clone https://github.com/zazaji/vespera.git
    cd vespera
    ```

3.  **Install frontend dependencies**:
    ```sh
    npm install
    ```

4.  **Run in development mode**:
    ```sh
    npm run tauri dev
    ```

### ❤️ Contributing

Contributions are welcome! Please feel free to submit an issue or pull request.

### 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.