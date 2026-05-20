# Euclid Data in the Cloud

Hands-on materials for the AAS 248 workshop **"Euclid Data in the Cloud: Access, Analysis, and Science Opportunities"** — Sunday June 14, 2026.

The workshop walks through how to pull Euclid Q1 data products out of the public IRSA cloud (TAP, SIA, direct S3 reads) and put them to work on real science: galaxy cluster identification and AGN selection. Total runtime is 90 minutes.

## Workshop arc

1. **`00_intro.pdf`** — slide deck. Mission overview, what Q1 is, data products and their acronyms (VIS, NISP, MER, SIR, SPE, PHZ), and what the next three notebooks do. *Not in this repo yet — distributed separately at workshop time.*
2. **[`01_Euclid_data_cloud.ipynb`](01_Euclid_data_cloud.ipynb)** — teaching notebook. Glossary of TAP / SIA / S3, then four worked examples: the MER photometric catalog (TAP), MER mosaic cutouts (SIA + direct S3), SIR 1D spectra (TAP + direct S3), and the SPE line-feature catalog (TAP). Closes with how to discover tables and columns yourself, and a pointer to HATS for population-scale work.
3. **[`02_Euclid_clusters.ipynb`](02_Euclid_clusters.ipynb)** — science notebook 1. Picks a high-richness Q1 galaxy cluster, fetches its photo-z-matched candidate members with an ADQL `JOIN` across MER and PHZ, runs DBSCAN against a control field 5′ away, and overlays the over-density on H-band imaging.
4. **[`03_Euclid_AGN_assignment.ipynb`](03_Euclid_AGN_assignment.ipynb)** — 30-minute hands-on. Projects ~5k EDF-N AGN candidates onto a pre-trained color-SOM, compares selection methods (WISE, Gaia, DESI, …) on the same manifold, and verifies one candidate with three live cloud reads (S3 spectrum, IBE imaging, TAP line catalog). [`03_Euclid_AGN_solutions.ipynb`](03_Euclid_AGN_solutions.ipynb) is the same notebook with every TODO filled in.

## Quick start

```bash
git clone <repo-url>
cd euclid-workshop
pip install -r requirements.txt
jupyter lab          # or: jupyter notebook
```

Open the notebooks in order. Everything except the AGN notebook's pre-trained SOM is fetched live from IRSA at run time, so you need network access during the workshop.

## Prerequisites

- **Python ≥ 3.10.** Tested against 3.12.
- **Packages** — `astropy`, `astroquery >= 0.4.10`, `minisom`, `numpy < 2`, `scipy < 1.13`, `matplotlib`, `s3fs`, `tqdm`, `ipywidgets`, `requests`, `scikit-learn`. All pinned in [`requirements.txt`](requirements.txt).
- **Network access** to `irsa.ipac.caltech.edu` (TAP, SIA, IBE) and `s3://nasa-irsa-euclid-q1` (anonymous S3). No AWS credentials required.

The same code runs unchanged from a laptop or from a NASA Fornax JupyterLab session; from Fornax (in `us-east-1`, the same AWS region as the bucket) the S3 transfers are substantially faster.

## What ships in the repo

| Path | Size | Purpose |
| --- | --- | --- |
| `data/cache/workshop_som.pkl` | 70 KB | Pre-trained Q1 color SOM (notebook 03). |
| `data/cache/workshop_train_edfn.fits` | 5.9 MB | Training-galaxy BMU positions (notebook 03). |
| `data/cache/workshop_agn_flags_edfn.fits` | 349 KB | EDF-N AGN candidate IDs, positions, redshifts, selection flags (notebook 03). |
| `data/cache/workshop_agn_edfn.fits` | 1.3 MB | Pre-projected fallback if Task 1's live TAP query fails. |
| `data/cache/irsa_spectra_agn/*.npz` | 68 KB | Local cache of SIR spectra for offline re-runs. |
| `AAS247/euclid_q1_clusters.csv` | 4 KB | Q1 cluster catalog (notebook 02). |

Total: ~8 MB. Everything else — MER photometry, mosaic imaging, SIR spectra, SPE line measurements — is pulled live from IRSA.

## References

- Q1 cloud-access tutorial (the original, long-form): https://caltech-ipac.github.io/irsa-tutorials/euclid-cloud-access/
- Q1 cluster tutorial: https://caltech-ipac.github.io/irsa-tutorials/euclid-clusters-tutorial/
- Q1 SPE catalog tutorial: https://caltech-ipac.github.io/irsa-tutorials/euclid-intro-spe-catalog/
- Q1 HATS / Parquet-on-S3 tutorial: https://caltech-ipac.github.io/irsa-tutorials/euclid-q1-hats-intro/
- IRSA holdings for Euclid: https://irsa.ipac.caltech.edu/Missions/euclid.html
- Q1 cluster catalog: Euclid Collaboration: Bhargava et al. 2025, [arXiv:2503.19196](https://arxiv.org/abs/2503.19196)
- Q1 AGN candidate catalog: Euclid Collaboration: Matamoro Zatarain et al. 2025
