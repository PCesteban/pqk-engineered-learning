# Engineered Learning Problems on Noisy Quantum Processors

Data and code for [arXiv:2610.41848](https://arxiv.org/abs/2610.41848).

Data: [10.5281/zenodo.24394890](https://doi.org/10.5281/zenodo.24394890)

**TL;DR:** download `data.zip` from Zenodo, unzip it here, `pip install -r requirements.txt` and run `reproduce.ipynb`
(about 9 minutes). It rebuilds Tables II–V and Figs. 3–7, 10 and 11, and recomputes a subset of the stored accuracies from
the raw data; it stops if any differs by 0.001 or more.

## Usage

Download `data.zip` from Zenodo and unzip it here, then

```bash
pip install -r requirements.txt
jupyter notebook reproduce.ipynb
```

Tested with Python 3.11 and 3.13.

The notebook rebuilds Tables II–V and Figs. 3, 4, 5, 6, 7, 10 and 11 from the stored accuracies, and runs the checks
below (about 9 minutes in total).

## What the notebook checks

The accuracies were computed with our own code, which is not public. The notebook recomputes some of them from the
released data, with only the code in the notebook, so you don't have to trust the stored numbers. Each check stops the
notebook if the recomputed and stored accuracy differ by 0.001 or more.

First, that every data file matches its SHA-256 in `MANIFEST.csv` (a corrupt or incomplete download stops here).

Computed from the data each time, not stored: the gaps, lower bounds and t-tests (Tables II and V), the choice of best
classical model, $c$, $\|r\|$, $\varepsilon$, $d_F$ and CKA (Table III, Fig. 4), and the $N_\mathrm{train}=400$ line in
Fig. 6.

Refit from features, inputs, labels and indices, and asserted equal to the stored accuracy (to 0.001):

- Table II: challenger at 15q $T=3$ and $T=10$; hist gbm at 15q $T=10$ (classical inputs and model code, Appendix A
  grids).
- Table III: noiseless kernel and Garnet (original run) on the ideal labels.
- Table IV: Garnet (original run) on the ZNE labels.
- MPS challenger at 45q $T=10$.
- Table V: challenger at $T=3$; random forest at $T=6$ (the best classical model there).
- Fig. 5: referee and challenger at $N=100$ and $550$. Fig. 6: challenger at $N_\mathrm{train}=50$, $200$ and $390$.

Then 5 random points, set by `SEED` under "Random checks" near the end of the notebook: a single run of any stored PQK
accuracy (Tables II–V, MPS), a single run of the best classical model behind each CV gap (Tables II and V), or one data
size of Fig. 5 or 6. Change `SEED` to check other points. We ran every one of these points before release; all pass.

Not refit, stored only: the test-tuned accuracies, the other CV-tuned classical models (none of them sets a CV gap),
and the classical curves of Figs. 5 and 6. Not in the release: label construction (the labels are given) and the
hardware runs (the features are given).

## Data

- `inputs/`: the 600 Fashion-MNIST points (PCA, 49 components). An $n$-qubit kernel uses the last $n-1$.
- `features/`, `kernels/`: projected features and kernels, `fashion_mnist_pqk_{features,kernel}_{n}q_{T}trotter_{run}.npy`.
  Runs: `e`/`g` = Emerald/Garnet; `chainA`/`chainB` = the two chains at 30 and 45 qubits; `_orig`, `zne_`, `rem_`,
  `apt_` (APT) for the error-mitigation runs; `IDEALSIM600` = noiseless; `MPS` = matrix product state.
- `labels/`, `labels_new/`: engineered labels per configuration (`labels_new/` uses Eq. 22). The key `15q` is $T=3$.
- `splits.npz`: subsample and train/test indices for the 100 runs (10 subsamples $\times$ 10 labellings).
- `datasize/`: data-size experiments (Figs. 5 and 6), 15 qubits, $T=3$. `referee.npz`: per size `N`, the subsample
  indices (first 2/3 train) and labels. `challenger.npz`: labels on all 600 points (test set 400–599) and the training
  indices per `N_train`.
- `accuracies/`: per-run test accuracies behind the tables and figures. `datasize.npz` holds the plotted mean and
  std per size for Figs. 5 and 6.
- `MANIFEST.csv`: SHA-256 of every file.

## Citation

```bibtex
@misc{payares2026engineeredlearningproblemsnoisy,
      title={Engineered Learning Problems on Noisy Quantum Processors}, 
      author={Esteban Payares and Nadia Milazzo and Ghada Ben Slama and Thomas Cope},
      year={2026},
      eprint={2610.41848},
      archivePrefix={arXiv},
      primaryClass={quant-ph},
      url={https://arxiv.org/abs/2610.41848}, 
}
```
