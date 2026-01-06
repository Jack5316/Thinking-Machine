---
title: "Using Claude Code in Mainland China: A Complete Guide with Proxy Setup"
date: 2026-01-06
draft: false
tags: ["Claude Code", "AI", "Development Tools", "China", "Proxy", "Tutorial"]
categories: ["Technical"]
---

### Introduction

As AI-powered development tools become increasingly essential for modern software development, accessing these tools from mainland China can present unique challenges due to network restrictions. **Claude Code**, Anthropic's official CLI for Claude, is a powerful tool that can significantly boost developer productivity—but it requires proper network configuration to work in China.

In this guide, I'll walk you through the complete process of setting up and using Claude Code in mainland China using **Clash for Windows** as a proxy solution and the **native Node.js** version of Claude Code.

---

### Why This Guide?

Many developers in mainland China face difficulties accessing AI services like Claude due to network restrictions. While VPN solutions exist, they're not always reliable or practical for development work. This guide provides a stable, reproducible solution using:

- **Clash for Windows**: A powerful proxy client with excellent performance
- **Native Node.js**: Direct installation without Docker complications
- **PowerShell proxy configuration**: Simple environment variable setup

---

### Prerequisites

Before we begin, make sure you have:

1. **Node.js** (v18 or later) - [Download from nodejs.org](https://nodejs.org/)
2. **Clash for Windows** - A proxy client for Windows
3. **An Anthropic API key** - Sign up at [console.anthropic.com](https://console.anthropic.com/)
4. **A working proxy subscription** - For Clash for Windows

---

### Step 1: Setting Up Clash for Windows

#### 1.1 Installation

1. Download Clash for Windows from its official source
2. Install and launch the application
3. Import your proxy subscription URL or configuration file

#### 1.2 Configuration

1. In Clash for Windows, go to the **General** tab
2. Note the **HTTP proxy port** (usually `7890`)
3. Enable **System Proxy** mode
4. Set the proxy mode to **Rule** or **Global** depending on your needs
   - **Rule mode**: Only routes specific traffic through proxy (recommended)
   - **Global mode**: Routes all traffic through proxy

#### 1.3 Verify Proxy is Working

Open your browser and visit [https://www.google.com](https://www.google.com) to verify your proxy is working correctly.

---

### Step 2: Installing Claude Code with Native Node.js

#### 2.1 Open PowerShell

Press `Win + X` and select **Windows PowerShell** or **Windows Terminal**.

#### 2.2 Set Proxy Environment Variables

Before installing Claude Code, configure PowerShell to use your proxy:

```powershell
$Env:http_proxy="http://127.0.0.1:7890"
$Env:https_proxy="http://127.0.0.1:7890"
```

**Important Notes:**
- Replace `7890` with your actual Clash HTTP proxy port if different
- These environment variables only persist for the current PowerShell session
- You'll need to set them again each time you open a new PowerShell window

#### 2.3 Install Claude Code via npm

With the proxy configured, install Claude Code globally:

```powershell
npm install -g @anthropic/claude-code
```

The installation should proceed smoothly through the proxy. If you encounter timeout errors, verify:
- Clash for Windows is running
- The proxy port number is correct
- System proxy is enabled in Clash

---

### Step 3: Configuring and Running Claude Code

#### 3.1 Set Your API Key

Before first use, configure your Anthropic API key:

```powershell
$Env:ANTHROPIC_API_KEY="your-api-key-here"
```

Alternatively, create a `.env` file in your project directory:

```
ANTHROPIC_API_KEY=your-api-key-here
```

#### 3.2 Running Claude Code

Each time you want to use Claude Code, follow these steps:

1. **Open PowerShell**
2. **Set proxy environment variables:**
   ```powershell
   $Env:http_proxy="http://127.0.0.1:7890"
   $Env:https_proxy="http://127.0.0.1:7890"
   ```
3. **Set API key** (if not using .env file):
   ```powershell
   $Env:ANTHROPIC_API_KEY="your-api-key-here"
   ```
4. **Navigate to your project directory:**
   ```powershell
   cd C:\path\to\your\project
   ```
5. **Start Claude Code:**
   ```powershell
   claude-code
   ```

---

### Step 4: Creating a PowerShell Profile for Convenience

Setting proxy variables every time can be tedious. Let's automate it with a PowerShell profile.

#### 4.1 Create or Edit Your Profile

Check if you have a profile:
```powershell
Test-Path $PROFILE
```

If it returns `False`, create one:
```powershell
New-Item -Path $PROFILE -Type File -Force
```

#### 4.2 Edit the Profile

Open the profile in your preferred editor:
```powershell
notepad $PROFILE
```

#### 4.3 Add Proxy Configuration

Add these lines to automatically set proxy variables:

```powershell
# Claude Code Proxy Configuration for China
function Set-ClaudeProxy {
    $Env:http_proxy="http://127.0.0.1:7890"
    $Env:https_proxy="http://127.0.0.1:7890"
    Write-Host "Proxy configured for Claude Code (127.0.0.1:7890)" -ForegroundColor Green
}

# Uncomment the next line to automatically set proxy on PowerShell startup
# Set-ClaudeProxy

# Create an alias for convenience
Set-Alias -Name claude-proxy -Value Set-ClaudeProxy
```

#### 4.4 Using the Profile

Now you can simply type:
```powershell
claude-proxy
```

This will set your proxy environment variables without typing the full commands.

---

### Step 5: Troubleshooting Common Issues

#### Issue 1: "Connection Timeout" or "ECONNREFUSED"

**Solution:**
- Verify Clash for Windows is running
- Check the proxy port in Clash settings (General tab)
- Ensure system proxy is enabled
- Try setting proxy variables again

#### Issue 2: "API Key Not Found"

**Solution:**
- Verify your API key is correct
- Check that the environment variable is set: `echo $Env:ANTHROPIC_API_KEY`
- Try setting it again in the current session

#### Issue 3: Slow Response Times

**Solution:**
- Switch to a faster proxy server in Clash
- Try changing from Rule mode to Global mode
- Check your internet connection speed

#### Issue 4: npm Install Fails

**Solution:**
- Set proxy before running npm install
- Try using npm with explicit proxy:
  ```powershell
  npm --proxy http://127.0.0.1:7890 --https-proxy http://127.0.0.1:7890 install -g @anthropic/claude-code
  ```
- Clear npm cache: `npm cache clean --force`

#### Issue 5: Proxy Settings Not Persisting

**Solution:**
- Proxy environment variables only last for the current PowerShell session
- Use the PowerShell profile method described in Step 4
- Or set system-wide environment variables in Windows Settings

---

### Best Practices

1. **Keep Clash Running**: Always ensure Clash for Windows is running when using Claude Code
2. **Use Rule Mode**: Configure Clash rules to only proxy international traffic for better performance
3. **Secure Your API Key**: Never commit your API key to version control
4. **Test Your Setup**: Before starting a coding session, verify your proxy with a simple test:
   ```powershell
   curl https://api.anthropic.com/v1/messages -H "x-api-key: $Env:ANTHROPIC_API_KEY"
   ```
5. **Document Your Port**: If you change Clash's proxy port, update your PowerShell profile accordingly

---

### Alternative: Using Docker with Clash

If you prefer using Docker, you can also configure Docker to use Clash as a proxy. However, the native Node.js approach is generally simpler and more performant for development work.

---

### Conclusion

Using Claude Code in mainland China is definitely achievable with the right setup. By combining Clash for Windows with proper PowerShell proxy configuration, you can enjoy seamless access to Claude's powerful AI coding capabilities.

The key points to remember:
- Always set proxy environment variables before using Claude Code
- Keep Clash for Windows running during your development session
- Use a PowerShell profile to automate the setup process
- Verify your proxy and API key configuration if you encounter issues

With this setup, you can now leverage Claude Code's capabilities for code review, refactoring, debugging, and much more—all from mainland China!

---

### Additional Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Clash for Windows GitHub](https://github.com/Fndroid/clash_for_windows_pkg)
- [Anthropic API Documentation](https://docs.anthropic.com/)
- [Node.js Downloads](https://nodejs.org/)

---

### Questions or Issues?

If you encounter any problems following this guide or have suggestions for improvements, feel free to reach out via [email](mailto:jiaweitan531@gmail.com) or connect with me on [GitHub](https://github.com/jack5316).

Happy coding with Claude!

**Jiawei Tan (Jack)**
*January 6, 2026*
