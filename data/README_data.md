# Dataset Instructions

This project uses the **NASA C-MAPSS** (Commercial Modular Aero-Propulsion System Simulation) dataset.

## Download

1. Go to the [NASA Prognostics Data Repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)
2. Find **"Turbofan Engine Degradation Simulation Data Set"**
3. Download and unzip the archive

## File Placement

Place the extracted `.txt` files in this `data/` folder:

```
data/
├── train_FD001.txt
├── test_FD001.txt
├── RUL_FD001.txt
├── train_FD002.txt
├── test_FD002.txt
├── RUL_FD002.txt
├── train_FD003.txt
├── test_FD003.txt
├── RUL_FD003.txt
├── train_FD004.txt
├── test_FD004.txt
└── RUL_FD004.txt
```

## Alternative: Kaggle

The dataset is also available on Kaggle:  
[https://www.kaggle.com/datasets/behrad3d/nasa-cmaps](https://www.kaggle.com/datasets/behrad3d/nasa-cmaps)

If running on Kaggle Notebooks, the files are already available at `/kaggle/input/nasa-cmaps/`.

## Notes

- Files are space-separated with no header row
- 26 columns per row: `[engine_id, cycle, op_1, op_2, op_3, s1 ... s21]`
- The dataset is **not redistributable** under NASA's terms, which is why it is excluded from this repository
