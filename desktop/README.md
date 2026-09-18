# DeepTune desktop user guide

[Download the app](https://github.com/ply971/deeptune-installer/releases/latest) · [GUI demo](../README.md#deeptune-gui-demo) · [Troubleshooting](../docs/TROUBLESHOOTING.md)

Use DeepTune to prepare a dataset, configure a model, run training, inspect the results, and predict on new data from a desktop interface.

## Find your way around

| Page | Use it for |
| --- | --- |
| **Overview** | See the workflow and jump to a dataset or results. |
| **Dataset** | Choose your data type and source, preview data, set the target column, and build video clip lists. |
| **Model & Training** | Choose a model, training preset, and settings. |
| **Run** | Choose an output folder, save or load settings, start or stop training, and follow logs. |
| **Results** | Browse experiments, inspect metrics and curves, and export metrics. |
| **Test** | Use a trained model on new data and inspect predictions. |

## Your first training run

### 1. Choose and preview your data

On **Dataset**, select `images`, `text`, `tabular`, `timeseries`, or `video`.

Leave **This is raw data (needs conversion to parquet)** enabled for ordinary folders, CSV/XLSX files, or a video clip list. Disable it when supplying an existing DeepTune-compatible parquet dataset.

Use **Browse folder** or **Browse file**, then **Load / preview**. A table preview shows up to 50 rows. Image and video class folders show class counts; individual video clips can be played in the preview.

| Data type | Training input |
| --- | --- |
| Images | A folder containing one subfolder per class, or a prepared parquet dataset. |
| Video | Class folders, a labeled clip list created in the app, a CSV/XLSX video manifest, or a prepared parquet dataset. |
| Text | A CSV/XLSX file containing text records and their target labels, or prepared parquet. |
| Tabular | A CSV/XLSX file with feature columns and a target column, or prepared parquet. |
| Time series | A CSV/XLSX file with the required time index, target, and model input columns, or prepared parquet. |

Set **Target column** to the value the model should learn to predict. The default is `labels`. For supported grouped datasets, **Grouper column** can identify related records, such as records from the same subject, so those groups stay together during splitting.

For image/video classification folders, organize your data like this:

```text
my-dataset/
  class-a/
    sample-01
    sample-02
  class-b/
    sample-03
    sample-04
```

Use real image or video filenames and enough examples in each class for training, validation, and evaluation. Folder-based inputs are for classification. Numeric-target regression needs prepared parquet, or a video manifest with numeric labels.

### 2. Choose a model and settings

Open **Model & Training**. **Model version** lists options for the selected data type.

Start with **Quick check: 1 epoch** to test a small dataset, or **Standard: 10 epochs** for a longer starting configuration. These presets set epochs, batch size, and backbone freezing. You can then adjust:

| Control | What it changes |
| --- | --- |
| **Number of classes** | How many categories an image, text, or video classifier should predict. |
| **cls / reg** | Classification (categories) or regression (numbers), where supported. |
| **Epochs** | How many training passes to make through the dataset. |
| **Batch size** | How many examples to process at once; smaller values use less memory. |
| **Learning rate** | The size of the model's training updates. |
| **Use a reproducible seed (42)** | Uses a fixed random seed for repeatable experiments. |
| **Freeze backbone** | Keeps the pretrained feature extractor fixed, where supported. |
| **Use PEFT / LoRA** | Enables parameter-efficient fine-tuning for compatible models. |
| **Added layers / Embedding size** | Adjusts the added model layers and feature representation, where supported. |

Extra controls appear for particular workflows: frame count and pooling for video, continuous/categorical feature lists for GANDALF, fine-tuning for TabPFN, and a time index column for time series. Text models support classification; GPT-2 does not support the PEFT option.

### 3. Start training

Open **Run**, choose an output folder, and read the readiness message. Resolve missing input or unsupported-setting messages before starting.

Click **Run**. Follow **Live output**, the run stage, and elapsed time. **Follow output** keeps the log at its newest messages. Use **Save log** for a separate copy, or **Stop** to cancel the run.

Closing the app stops active work. Stopped or failed runs may leave partial output; a partial folder does not mean a usable model has been saved.

### 4. Review results

Open **Results**, click **Refresh**, and select an experiment from **Run history**.

- **Holdout metrics** shows the evaluation values available for that run.
- **Training curve** plots training and validation loss when those values were recorded.
- **Paths** identifies available model checkpoints and embedding files.
- **Export metrics CSV** exports the selected experiment's available metrics.
- **Open selected folder** opens the experiment files.

The default data split is 70% training, 10% validation, and 20% holdout. Classification tries to preserve class proportions; grouped splits keep groups together, and time-series splits preserve time order. Very small datasets may not support the requested split.

## Import MP4 videos

1. On **Dataset**, select **video** with **Raw data** enabled.
2. Click **Select MP4 / video files...**. Use Ctrl/Shift to choose several clips.
3. Preview the selected clip. **Play/Pause**, **Restart**, **Loop**, and the seek bar control playback. **Preview** beside a clip plays that clip without replacing your training dataset.
4. Enter each clip's class label, or use **Apply label to selected** to label the current selection together.
5. Click **Use clip dataset**. The app saves a CSV list and selects it for training.
6. Choose the model and training options, then start from **Run**.

Labels are needed for training, but you can import and preview unlabeled clips. Aim for at least ten clips per class as a useful starting point for splitting; this is not a guarantee of model quality. Keep the original videos available at their saved paths.

The inline preview is muted and shows frame rate, dimensions, and duration. **Open in video player** opens your system player for audio or full-screen playback. Preview playback pauses when you leave the Dataset page.

### Use an existing video list

A CSV/XLSX manifest needs `videos` and `labels` columns for training. Relative video paths are resolved from the manifest's folder. Duplicate files, missing paths, and missing labels are reported.

```csv
videos,labels
clips/walking-01.mp4,walking
clips/running-01.mp4,running
```

This example shows the column format; add enough clips in each class before training. For regression, provide numeric target labels instead of class names.

Accepted video containers include MP4, AVI, MOV, MKV, WebM, M4V, WMV, MPEG, and MPG. Playback also depends on the codec inside the file.

### Choose a video analysis approach

**Frame-based models** sample frames across each clip and combine their features with **mean** or **attention** pooling. **Native video models**, such as `r3d_18` or `swin3d_t`, process the frame sequence together; the pooling choice does not apply to them. The MViT option requires 16 sampled frames.

Video analysis predicts one result for a whole clip. It uses visual content, not audio, and does not provide object tracking or event timestamps.

## Test new data

1. On **Run**, select the output folder containing the experiment you want to use.
2. Open **Test** and click **Refresh**.
3. Select a trained run with a saved checkpoint. The app restores the model settings from that run.
4. Under **New data**, browse to an image, a video, an image/video folder, or a supported data file.
5. Click **Test model**. Follow the live output, then inspect **Metrics** and **Predictions**.
6. Click **Open output folder** for the full prediction files.

For image/video folders, class subfolders supply known labels; a flat folder lets you predict without labels. Video inference also accepts CSV/XLSX manifests without a `labels` column. For tables, text, and time series, use the columns expected by the trained model.

| Output | What you get |
| --- | --- |
| **Predicted class or value** | A category for classification or a number for regression. |
| **Confidence** | A classification score alongside the prediction. |
| **Comparison to known labels** | Correctness or numeric error when ground truth is supplied. |
| **predictions.csv** | The complete per-sample results; the app previews up to 200 rows. |
| **inference_metrics.json** | Summary metrics when labeled test data is available. |

Unlabeled inputs produce predictions without accuracy metrics. **Stop** cancels an active test.

Current support limits: SigLIP image models are not wired into the Test page. Time-series testing needs history from the original run's saved data splits; it is not an unrestricted future-forecasting tool. Some TabPFN models also require external model access. See [Help](../docs/TROUBLESHOOTING.md#current-app-limits).

## Save settings and return later

Use **Save settings...** on Run to store a reusable configuration, and **Load settings...** to restore it. The app also restores its previous session when reopened.

Each launch saves its configuration and full logs under `desktop-sessions` in the chosen output folder. Keep datasets at the paths recorded in your settings, and keep saved experiment folders intact for later testing.

## Need help?

[Troubleshooting](../docs/TROUBLESHOOTING.md) · [Report an app issue](https://github.com/ply971/deeptune-installer/issues) · [Installation and download](../README.md#install-and-open)
