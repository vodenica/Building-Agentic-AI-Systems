### Run a Jupyter Notebook (`*.ipynb`) on Your Local Machine


#### Run Virtual Environments with Jupyter!**

As someone working in ed tech and creating courses, using virtual environments with Jupyter notebooks is **definitely possible and highly recommended** for managing project dependencies This is especially useful when you're working on multiple projects or course materials that require different package versions.

#### **Using venv with Jupyter**

Here's how to set up a virtual environment for your Jupyter notebook:

1. **Create a virtual environment:**
   ```bash
   python3 -m venv myenv
   ```

2. **Activate the environment:**
   - Windows: `myenv\Scripts\activate`
   - Mac/Linux: `source myenv/bin/activate`

3. **Install Jupyter and ipykernel in the virtual environment:**
   ```bash
   pip install jupyter ipykernel
   ```

4. **Add your virtual environment as a Jupyter kernel:**
   ```bash
   python -m ipykernel install --user --name=myenv --display-name="My Virtual Env"
   ```

5. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```

Now when you create a new notebook or open an existing one, you can select your virtual environment from the **Kernel > Change kernel** menu . Your virtual environment will appear in the list of available kernels .

### Run without virtual environment

**Step 1: Install Jupyter Notebook**

If you haven’t already, install Jupyter Notebook using pip:

```
pip install notebook
```

Or, if you use Anaconda, Jupyter is included by default.

---

**Step 2: Launch Jupyter Notebook**

Open your terminal (or command prompt), navigate to the folder containing your `.ipynb` file, and run:

```
jupyter notebook
```

This command will start a local server and open the Jupyter Notebook dashboard in your web browser, usually at `http://localhost:8888`. From there, you can click on your `.ipynb` file to open and run it interactively .

---

**Step 3: Run All Cells in the Notebook**

Once your notebook is open in the browser:
- Use the menu: **Cell > Run All** to execute all cells.
- Or, run cells one by one using **Shift + Enter**.

---

**Alternative: Run Notebook from Terminal (Non-Interactive)**

If you want to execute the notebook straight from the terminal (without opening the browser), you can use:

```
jupyter nbconvert --to notebook --execute your_notebook.ipynb
```

This will run all cells and save the output in a new notebook file .

---

**Summary Table**

| Task                        | Command/Action                                      |
|-----------------------------|-----------------------------------------------------|
| Install Jupyter             | `pip install notebook`                              |
| Start Jupyter server        | `jupyter notebook`                                  |
| Open notebook in browser    | Click `.ipynb` file in dashboard                   |
| Run all cells (browser)     | Menu: Cell > Run All or use `Shift + Enter`         |
| Run notebook from terminal  | `jupyter nbconvert --to notebook --execute file.ipynb` |

---

Let me know if you need help with installation, troubleshooting, or running notebooks in a specific environment!