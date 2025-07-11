# 💡 Forward-thinking tips

✅ **Use Miniconda** (lighter than Anaconda) if you only want the manager, not 500 packages pre-installed.
✅ Keep **separate environments per project** — avoid “big base” with everything installed.
✅ Clean up old environments occasionally.
✅ Document your environment with `environment.yml` — it’s professional, reproducible, and future-proof.

---

# 🔥 Quick summary table

| Task            | Command Example                     |
| --------------- | ----------------------------------- |
| Create env      | `conda create -n myenv python=3.10` |
| Activate env    | `conda activate myenv`              |
| Deactivate      | `conda deactivate`                  |
| **List envs**       | `conda env list`                    |
| Install package | `conda install numpy`               |
| Remove env      | `conda remove -n myenv --all`       |
| Export env      | `conda env export > env.yml`        |
| Import env      | `conda env create -f env.yml`       |

---