Here is the full guide in .md format, presented as requested with all steps and information included:

# Configuring SSH for Personal and Work GitHub Accounts

This guide provides a step-by-step explanation on how to set up and manage SSH keys for both **personal** and **work** GitHub accounts. By following this configuration, you will avoid permission issues and work seamlessly across multiple repositories using distinct GitHub accounts.

## Step 1: Generate SSH Keys

You need to generate **two separate SSH keys**: one for **personal use** and one for **work use**.

### Personal SSH Key

1. Open your terminal and run the following command to generate a personal SSH key:

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your.personal.email@example.com"
   ```

   Replace `"your.personal.email@example.com"` with your **personal GitHub email**.

2. When prompted to "Enter file in which to save the key," type:

   ```bash
   /Users/yourusername/.ssh/id_rsa_personal
   ```

   Replace `yourusername` with your system's username.

3. Press `Enter` to confirm. You can also choose to set a passphrase for additional security, or press `Enter` to leave it empty.

### Work SSH Key

4. Now, generate your **work SSH key**:

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your.work.email@example.com"
   ```

   Replace `"your.work.email@example.com"` with your **work GitHub email**.

5. When prompted to "Enter file in which to save the key," type:

   ```bash
   /Users/yourusername/.ssh/id_rsa_work
   ```

   Replace `yourusername` with your system's username.

6. Press `Enter` to confirm. Again, you can choose to set a passphrase for extra security or leave it empty.

## Step 2: Add SSH Keys to the SSH Agent

### Start SSH Agent

1. Ensure the SSH agent is running. In your terminal, type:
   ```bash
   eval "$(ssh-agent -s)"
   ```

### Add the Personal SSH Key

2. Add your **personal SSH key** to the SSH agent:
   ```bash
   ssh-add ~/.ssh/id_rsa_personal
   ```

### Add the Work SSH Key

3. Add your **work SSH key** to the SSH agent:
   ```bash
   ssh-add ~/.ssh/id_rsa_work
   ```

## Step 3: Add SSH Keys to GitHub Accounts

### Personal GitHub SSH Key

1. Copy your **personal public SSH key**:

   ```bash
   cat ~/.ssh/id_rsa_personal.pub
   ```

   Copy the entire output.

2. Go to your **Personal GitHub** account, navigate to **Settings** > **SSH and GPG keys** > **New SSH key**. Paste the copied public key into the key field and save it.

### Work GitHub SSH Key

3. Copy your **work public SSH key**:

   ```bash
   cat ~/.ssh/id_rsa_work.pub
   ```

   Copy the entire output.

4. Go to your **Work GitHub** account, navigate to **Settings** > **SSH and GPG keys** > **New SSH key**. Paste the copied public key into the key field and save it.

## Step 4: Configure SSH for Multiple GitHub Accounts

You need to set up an SSH configuration file to manage the multiple SSH keys for your **personal** and **work** GitHub accounts.

1. Open the SSH config file (create it if it doesn't exist):

   ```bash
   nano ~/.ssh/config
   ```

2. Add the following configurations for both personal and work accounts:

   - For **Personal GitHub**:

   ```bash
   Host github.com-personal
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_personal
       IdentitiesOnly yes
   ```

   - For **Work GitHub**:

   ```bash
   Host github.com-work
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_work
       IdentitiesOnly yes
   ```

3. Save the file by pressing `CTRL+X`, then `Y`, and `Enter`.

## Step 5: Test SSH Configuration

To ensure that everything is set up correctly, run the following tests for both your **personal** and **work** GitHub accounts.

### Test Personal SSH Key

```bash
ssh -T git@github.com-personal
```

You should see:

Hi yourusername! You've successfully authenticated, but GitHub does not provide shell access.

Test Work SSH Key

```bash

ssh -T git@github.com-work
```

You should see:

Hi yourworkusername! You've successfully authenticated, but GitHub does not provide shell access.

## Step 6: Update Your Git Remote URLs

You need to use the correct SSH URL for each repository.

### For Personal Repos: 1. If you’re cloning a personal repository, use the following SSH URL:

```bash

git@github.com-personal:yourusername/your-repo.git
```

### For Work Repos: 2. If you’re cloning a work repository, use the following SSH URL:

```bash
git@github.com-work:yourworkusername/your-repo.git
```

**To change the remote URL for an existing repo:**

```bash
git remote set-url origin git@github.com-personal:yourusername/your-repo.git # For personal repos

OR

git remote set-url origin git@github.com-work:yourworkusername/your-repo.git # For work repos
```

## Step 7: Verify Configuration

Make sure that your Git configuration is correct for each repository: 1. For a personal repo, run:

    1.	For a personal repo, run:

```bash
git config user.name
git config user.email
```

    2.	For a work repo, run:

```bash
git config user.name
git config user.email
```

Each repository should use the appropriate credentials and SSH keys based on the configuration.

**Recap of Key Steps:**

1. Generate separate SSH keys for your personal and work accounts.
2. Add both SSH keys to the SSH agent.
3. Configure the SSH config file to distinguish between personal and work accounts using custom hostnames (github.com-personal and github.com-work).
4. Test SSH keys to make sure both accounts are working properly.
5. Update the repository remote URLs to use the correct SSH configuration.

By following this setup, you’ll be able to work with both your personal and work GitHub repositories seamlessly, without encountering permission issues.
