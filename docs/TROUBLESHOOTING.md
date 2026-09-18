# DeepTune app help

[Download](https://github.com/ply971/deeptune-installer/releases/latest) · [User guide](../desktop/README.md) · [Back to the app overview](../README.md)

## Installation

**Do I need Python?** No. The Windows installer includes Python and the app's dependencies. Open DeepTune from the Start menu or desktop shortcut after setup.

**Which Windows versions are supported?** Windows 10 and 11, 64-bit. The installer installs for the current user and does not require administrator rights.

**How much disk space and memory do I need?** Allow several GB for the installed app, with additional space for datasets, downloaded model weights, checkpoints, and results. Small-model workflows can start around 8 GB RAM; 16 GB or more is more practical for larger workloads.

**Does the installer use my GPU?** This installer uses CPU builds of the model libraries. It does not use CUDA even when an NVIDIA GPU is present. Large models and video datasets can take substantial time on CPU.

**Does it need internet?** Some models download pretrained weights on first use. Cached weights can be reused later. Models that require access approval, such as some TabPFN weights, need the corresponding external account access.

**Why does Windows show “Unknown publisher”?** The installer is not code-signed. Check that you downloaded it from this repository's release page before deciding whether to proceed.

## Dataset and video import

| What you see | What to check |
| --- | --- |
| Training is not ready | Read the message on **Run**. Check the input path, data type, target column, class count, and required model options. |
| Too few examples or classes | Add more labeled examples to each class. Classification needs enough data for training, validation, and holdout evaluation. |
| Imported videos cannot become a dataset | Every clip needs a label before **Use clip dataset** becomes available. |
| A saved clip list no longer loads | Check that the original videos are still at the paths saved in the list. |
| A video cannot be decoded | Check that the file is not corrupt and that its codec is supported. The container extension alone does not guarantee playback. |
| No sound in the preview | The inline preview is muted. Use **Open in video player** for audio. |
| Regression is rejected for class folders | Use prepared parquet with numeric targets, or a video CSV/XLSX manifest with numeric labels. |

## Training and results

**The first run appears slow.** Check the live output for model downloads or training progress. Try a small dataset and **Quick check: 1 epoch** first. A larger frame count or model increases video processing work.

**A run runs out of memory.** Reduce the batch size and choose a smaller model. For video, consider fewer sampled frames where the selected architecture permits it.

**A model download fails.** Read the live log for connection or access errors. Restricted models need the relevant model access and authentication to be available; the app does not open a login prompt automatically.

**Results are empty or incomplete.** Select the correct output folder on Run, open Results, and click Refresh. A stopped or failed experiment can have partial files without metrics or a usable checkpoint. Inspect the run log.

**A metric or curve is missing.** The Results page displays what the selected run recorded. Different models produce different metrics, and partial runs may not have a complete training curve.

## Testing a saved model

**No trained runs appear.** On Run, select the folder containing your completed experiments. Return to Test and click Refresh.

**Test model is unavailable.** Select a run with a saved checkpoint. A partial training run may not have one.

**There are predictions but no accuracy score.** Accuracy and error metrics need known labels to compare against. Unlabeled data produces predictions only.

**Only some predictions are visible.** The app previews up to 200 rows. Use Open output folder to read the complete `predictions.csv`.

## Current app limits

- Text models support classification, not text generation or text regression.
- SigLIP image models cannot currently be used through the Test page.
- Time-series testing relies on history from the original run's saved splits. Keep those files with the experiment; arbitrary forecasting beyond the available history and inputs is not supported by this flow.
- Video predictions describe whole clips. Audio analysis, object tracking, and event timestamps are not provided.
- Model controls vary by data type and architecture. GPT-2 does not offer PEFT; MViT video models require 16 sampled frames.
- A short successful run confirms execution, not prediction quality. Evaluate with suitable labeled data before relying on a model's output.

## Report an app issue

[Open an issue](https://github.com/ply971/deeptune-installer/issues) with the app version, Windows version, page, data type, selected model, steps to reproduce, and the relevant error message. The **Save log** button and automatic session logs can help explain a failed training run.
