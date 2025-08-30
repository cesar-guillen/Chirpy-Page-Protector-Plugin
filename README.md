# Chirpy Page Protector Plugin

This plugin encrypts selected pages of your Chirpy-based Jekyll site and displays a password modal when visitors try to access them. The encryption is applied after the site has been generated, meaning your markdown files remain in plaintext locally, you must keep them private yourself.

## Features

🔒 AES-256-CBC encryption with HMAC-SHA256 integrity checks

🖼️ User-friendly modal that prompts for a password

⚡ Client-side decryption using the Web Crypto API

📝 Protects only posts in the Active category (configurable by editing the script)

🎯 Works with Chirpy theme’s generated _site content

⚠️ Important Notes

Markdown files are NOT encrypted.
The plugin only encrypts HTML content inside _site/ after site generation.
→ You must keep your source .md files private (e.g., in a private repo).


## Installation & Usage for Github Pages

1. Copy the Ruby script into your project (e.g., encrypt_posts.rb).
2. Ensure you have the required gems installed:
```bash
gem install nokogiri
```
3. Modify `.github/workflows/pages-deploy.yml` to include the code below after the site is built:
```yml
- name: Run protector
  run: bundle exec ruby _plugins/protector.rb
  env:
    PROTECTOR_PASSWORD: ${{ secrets.PROTECTOR_PASSWORD }}
```
5. Set the `PROTECTOR_PASSWORD` enviromnment variable in `Settings → Secrets and variables → Actions -> New repository secret`
6. You can now deploy the page like always.

Remember to set the posts you would like to protect by adding `Protect` in the Categories section 

## How It Works

* On build, the plugin:
1. Looks for posts marked with the Active category link.
2. Extracts the .content block from the post.
3. Encrypts it with AES-256-CBC using the password.
4. Replaces the content with a modal + decryption script.

* On the client side:
1. Visitors see a lock modal.
2. They enter the password.
3. The Web Crypto API verifies the HMAC and decrypts the content.
4. The post content is displayed dynamically.

## Demo UI (Modal)

When users access a protected post, they’ll see:
![example](images/modal.png)

Users will still be able to read a small preview form the home page 
![example](images/home.png)

