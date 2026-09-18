# DeepTune
## **Windows installer:** [https://github.com/ply971/deeptune-installer/releases/latest](https://github.com/ply971/deeptune-installer/releases/latest)

**Your datasets. Your models. One desktop workspace.**

DeepTune is a Windows desktop app for training models, reviewing results, and making predictions on new data. Import images, text, tables, time series, or video clips, then work through the app's pages without writing commands.

[![Latest release](https://img.shields.io/github/v/release/ply971/deeptune-installer)](https://github.com/ply971/deeptune-installer/releases/latest)
![Windows 10 / 11](https://img.shields.io/badge/Windows-10%20%2F%2011-0078D4)
![Desktop GUI](https://img.shields.io/badge/Interface-Desktop%20GUI-3D91EB)

[GUI demo](#deeptune-gui-demo) · [App features](#what-you-can-do-in-the-app) · [Install](#install-and-open) · [User guide](desktop/README.md)

## DeepTune GUI Demo

[![Tour of the DeepTune desktop app](docs/assets/deeptune-gui-demo.gif)](https://github.com/ply971/deeptune-installer/blob/main/docs/assets/deeptune-gui-demo.mp4)

**[Watch the 17-second GUI walkthrough](https://github.com/ply971/deeptune-installer/blob/main/docs/assets/deeptune-gui-demo.mp4)**

See the actual app's Overview, video import and playback, Model & Training, and Test pages. The tour uses a synthetic video clip and shows the interface; it does not show a completed training run.

## What you can do in the app

| Feature | What you can do |
| --- | --- |
| **Import and preview datasets** | Browse for folders or data files, select a data type, inspect table previews or class counts, and choose the column the model should predict. |
| **Build a video dataset** | Select multiple clips, preview them inside the app, assign labels, and save the clip list for training. |
| **Configure a model visually** | Choose a compatible model from a dropdown and adjust training settings, with extra options shown for your selected data type. |
| **Start with a preset** | Use **Quick check: 1 epoch** to try the workflow, or **Standard: 10 epochs** as a starting configuration. |
| **Control a training run** | Pick an output folder, check readiness messages, start a run, follow live output and elapsed time, or stop it. |
| **Review your experiments** | Browse run history, inspect available evaluation metrics and loss curves, find saved model files, and export metrics to CSV. |
| **Test new data** | Select a trained run, supply a new file or dataset, and see predictions. Classification results include confidence scores; labeled test data also provides evaluation metrics. |
| **Reuse your setup** | Save and load settings, restore the previous session, and keep the configuration and full logs for each launched run. |

### Work with five data types

| In the app | Typical workflow |
| --- | --- |
| **Images** | Train on labeled image folders, then classify new images. Numeric-target regression is available with prepared data and compatible models. |
| **Text** | Train a text classifier and predict categories for new text records. |
| **Tabular** | Use spreadsheet-style data to predict a category or numeric value. |
| **Time series** | Train a forecasting model using time-ordered records and inspect its results. Testing requires the original run's history files. |
| **Video** | Train on labeled clips, then predict a category or numeric value for new videos. |

The app updates its model choices and controls to match the selected data type. See the [user guide](desktop/README.md) for inputs and current support details.

## Install and open

1. Open the [latest release](https://github.com/ply971/deeptune-installer/releases/latest).
2. Download **DeepTune-Setup-1.1.0.exe** from **Assets**.
3. Run the installer and follow the setup steps.
4. Open **DeepTune** from the Start menu or desktop shortcut.

**Python and the app's dependencies are included.** You do not need to set up a Python environment or use a terminal.

The installer supports **Windows 10/11, 64-bit** and runs models on the **CPU**. Allow several GB of disk space for the app, plus room for your datasets and results. Some models need internet access to download weights on first use. For installation questions, see [Help](docs/TROUBLESHOOTING.md#installation).

## Your first training run

1. **Dataset** — choose your data type, browse to your data, and click **Load / preview**. For class folders, each subfolder represents a category.
2. **Model & Training** — pick a model and start with **Quick check: 1 epoch**. Review the class count and any data-specific settings.
3. **Run** — choose where to save results, resolve any readiness messages, and click **Run**.
4. **Results** — click **Refresh**, select your experiment, and inspect the available metrics and training curve.
5. **Test** — select the trained run, browse to new data, and click **Test model**.

A one-epoch run checks that your setup works. Use a suitable dataset and training configuration before judging model quality. Follow the [full desktop walkthrough](desktop/README.md#your-first-training-run) for more detail.

## Video analysis in the GUI

Import clips with **Select MP4 / video files...** and watch them directly in the app. The preview offers **play/pause, restart, looping, and seeking**, along with duration, dimensions, and frame rate. Use **Open in video player** when you want audio.

Assign a label to each clip, such as `walking` or `running`, and click **Use clip dataset**. Then choose how the model should analyze the video:

- **Frame-based models** combine information from sampled frames. You can select mean or attention pooling.
- **Native video models** process frame sequences to learn appearance and motion together.

After training, use **Test** to analyze new clips and view their predictions. Video predictions apply to the whole clip; the app does not draw tracking boxes or produce event timestamps. The model uses visual frames, not audio.

[Follow the video walkthrough →](desktop/README.md#import-mp4-videos)

## Predictions and exports

Use **Test** to select a saved model and predict on new data. View the predicted class or value, classification confidence, and evaluation metrics when known labels are available. **Open output folder** gives you the full **predictions.csv**.

Use **Results** to inspect training experiments and **Export metrics CSV** to save their metrics. **Save settings...** and **Load settings...** let you reuse a setup later.

Need a walkthrough? Open the [desktop user guide](desktop/README.md). Found a problem? [Report an app issue](https://github.com/ply971/deeptune-installer/issues).
