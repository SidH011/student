# Setting Up Your Python Environment by Using `venv` and `make`

## 1. Creating a Python Virtual Environment (`venv`)

A virtual environment helps isolate your Python dependencies for a project. To create one:

1. Open your terminal.
2. Navigate to your project directory:
   ```bash
   cd /home/sidh001/student
   ```
3. Create a virtual environment named `venv`:
   ```bash
   python3 -m venv venv
   ```
   This will create a folder called `venv` in your project directory.

## 2. Activating the Virtual Environment

To activate the virtual environment, run:
```bash
source /home/sidh001/student/venv/bin/activate
```
After activation, your terminal prompt will change, indicating that you are now using the virtual environment.

## 3. Using the `make` Command

The `make` command automates tasks defined in a file called `Makefile` (usually in your project root). Common tasks include installing dependencies, running tests, or building your project.

**Example usage:**
```bash
make install
```
This will run the `install` task defined in your `Makefile`.

**Note:** Always activate your virtual environment before running `make` if your tasks depend on Python packages.

## 4. Troubleshooting

### If `make` is not found:
- Install it using:
  ```bash
  sudo apt-get update
  sudo apt-get install make
  ```

### If `venv` is not found or fails to activate:
- Ensure you have Python 3 installed:
  ```bash
  python3 --version
  ```
- If `venv` is missing, install it:
  ```bash
  sudo apt-get install python3-venv
  ```
- If activation fails, check the path and permissions.

### If Python packages are missing after activating `venv`:
- Install them using `pip`:
  ```bash
  pip install -r requirements.txt
  ```

### If `make` tasks fail:
- Check your `Makefile` for typos or missing dependencies.
- Run the command manually to see detailed errors.

---

**Summary:**  
1. Create your virtual environment with `python3 -m venv venv`.
2. Activate it using `source /home/sidh001/student/venv/bin/activate`.
3. Use `make` to automate tasks.
4. Refer to troubleshooting steps if you encounter

## 5. Commands to Know

Here are some essential Linux commands, their functionality, usage, and how they relate to publishing on GitHub Pages:

| Command | Functionality | Usage Example | Relevance to GitHub Pages |
|---------|--------------|--------------|--------------------------|
| `cd`    | Change directory | `cd /home/sidh001/student` | Navigate to your project folder to manage or publish your site |
| `ls`    | List files and directories | `ls` | View files in your project, such as `_posts`, `Makefile`, etc. |
| `pwd`   | Print working directory | `pwd` | Confirm your current location in the filesystem before running commands |
| `cp`    | Copy files or directories | `cp file.md backup.md` | Backup important files before making changes |
| `mv`    | Move or rename files | `mv draft.md post.md` | Organize or rename blog posts before publishing |
| `rm`    | Remove files or directories | `rm old.md` | Clean up obsolete files from your project |
| `cat`   | Display file contents | `cat README.md` | Quickly view documentation or configuration files |
| `nano` / `vim` | Edit files in terminal | `nano _config.yml` | Make quick edits to your site configuration |
| `git`   | Version control | `git add .`, `git commit -m "Update"`, `git push`, `git pull`| Track changes and publish your site to GitHub Pages, also be able to pull from the directory you forked from|

**How these commands relate to publishing on GitHub Pages:**
- You use `cd`, `ls`, and `pwd` to navigate and manage your project files.
- `cp`, `mv`, and `rm` help organize and maintain your blog content.
- `cat`, `nano`, and `vim` allow you to view and edit files directly.
- `git` is essential for pushing your changes to GitHub, which triggers the publishing of your site via GitHub Pages.