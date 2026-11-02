# Basira Field Test

A one-page web app to test Basira's shelf detector on a phone. Open it, photograph a shelf, and the model boxes every product and counts products per shelf. It all runs **in the browser** with [onnxruntime-web](https://onnxruntime.ai). No server is involved, and photos never leave the device.

**Live:** https://kami-abdou.github.io/basira-field-test/

**Labelling tool:** https://kami-abdou.github.io/basira-field-test/label/. Open your own shelf photos and correct the boxes plus brand per product, then download the labels to measure accuracy. Photos stay on your computer; `label/prelabels.json` holds only box coordinates.

## The model

| | |
|---|---|
| Architecture | DETR, ResNet-50 backbone, one class ("product") |
| Weights | [`is36e/detr-resnet-50-sku110k`](https://huggingface.co/is36e/detr-resnet-50-sku110k) (Apache-2.0), fine-tuned on SKU-110K |
| Format | ONNX, dynamic uint8 per-channel quantization, 43 MB (fp32 is 167 MB) |
| Input | Short side 800 px (long side max 1333), ImageNet normalization |
| Accuracy | F1 0.79 / product-count error 6.8% on 20 real SKU-110K test photos (the fp32 model scores F1 0.81) |

**Notes:**
- SKU-110K was released for research use. This page is a technical demo, not a commercial product.
- The model finds products. Recognizing brands and SKUs needs a customer catalog and runs in the full Basira service.
- The sample shelf photo is from [Unsplash](https://unsplash.com).

## Run locally

```bash
python -m http.server 5600
```

Then open http://localhost:5600.
