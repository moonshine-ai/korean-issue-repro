# Repro Case for our Optimum Issue with Korean

This is a minimal reproducible example of a bug we're hitting with when converting Moonshine models to Onnx using Optimum.

In our situation this is only happening with Korean, so we're trying to track down what's going wrong. It's possible we're doing something wrong in our onnx-moonshine package that only shows up with non-English characters?

## Installation

```bash
pip install -r requirements.txt
```

## Downloading and Converting Models

Here's the script and args I'm using to get the ONNX version of the Korean Moonshine Models.

```bash
./download-moonshine-model.sh base ko
```

Internally it's calling `optimum-cli export onnx --model ...`.

## Reproducing the Problem

This repo includes an evaluation script that uses the Fleurs test set to calculate word and character error rates.

If you run this command it will print out the CER for Korean using Transformers:

```bash
python eval-moonshine-model.py --framework transformers --language ko_kr
CER: 9.04%
```

This error rate is what we'd expect. However if you run the same script using the ONNX versions of the models, you'll see a more worse accuracy score:

```bash
python eval-moonshine-model.py --framework onnx --language ko_kr
CER: 27.31%
```

Interestingly this doesn't happen with the English base models:

```bash
python eval-moonshine-model.py --framework onnx --language en_us
WER: 11.36%
```

```bash
python eval-moonshine-model.py --framework transformers --language en_us
WER: 11.36%
```