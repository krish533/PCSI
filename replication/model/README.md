# Classifier artifacts

`srn_cls_model/` contains everything needed to load the classifier except the weights:

- `config.json`, `label_map.json`: model architecture and class labels (restrictive, neutral, supportive)
- `tokenizer.json`, `tokenizer_config.json`, `special_tokens_map.json`, `vocab.txt`: tokenizer
- `temperature_scaling.json`: calibration parameter (T = 0.8462)
- `metrics.json`: training-time results (2,458 training and 274 validation sentences; accuracy 0.901; macro-F1 0.894)

## Weights

The weights file `model.safetensors` (about 438 MB) is not copied here, to keep the repository small. It is stored with Git LFS in the archived development repository. To retrieve it:

```bash
git clone https://github.com/krish533/Tech-transfer-1
cd Tech-transfer-1
git lfs pull --include "P1_replication_package/model/srn_cls_model/model.safetensors"
cp P1_replication_package/model/srn_cls_model/model.safetensors /path/to/PCSI/replication/model/srn_cls_model/
```

The weights are needed only to re-score sentences (`pipeline/run_all.py` without `--skip-scoring`, or `paper_outputs/code/verify_inference_reproducibility.py`). All tables and figures can be reproduced from the released scores without them.
