

### Initial Setup

1. **prepraing the Required Files**:
- We refer readers to our previous project, in which we used exactly the same dataset and provided a detailed explanation of the signal preprocessing steps and the creation of the required files. Please refer to our paper and the accompanying GitHub code: Moulaeifard, Mohammad, et al. “Machine-learning for photoplethysmography analysis: Benchmarking feature, image, and signal-based approaches.” Biomedical Signal Processing and Control 120 (2026): 109831. https://doi.org/10.1016/j.bspc.2026.109831

2. **Complete the Final Folder**:
   - Place the `mean.npy`, `lbl_itos.npy`, and `std.npy` files into the new folder created in step 1.
   - This folder should now contain the following six files:
     - `df_memmap.pkl`
     - `memmap.npy`
     - `memmap_meta.npz`
     - `mean.npy`
     - `lbl_itos.npy`
     - `std.npy`

3. **Specify Path for Use**:
   - When you need to access the dataset, specify the path to this final folder using the `--data` flag in the terminal.

# Examples for processing the datasets 


| key        | value                                                                                                                                                                       |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dataset    | pulsedb_calibfree_vital                                                                                                                                                                   |
| model      | xresnet1d50_MCD|
| script     | .../required_codes_files/main_ppg.py                     |
|train command    | `python main_ppg.py --data ./path/to/folder/with/six/final/files --input-size 1250 --architecture xresnet1d50_MCD --finetune-dataset pulsedb_calibfree_vital  --select-input-channel 0 --refresh-rate 1 --dr 0.4 --batch-size 512 --epoc 50 --normalize --seed $SEED`|
| comment    |   you may need to change the  SEED number as well as droupout rate (--dr)    |


