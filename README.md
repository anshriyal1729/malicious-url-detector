# Malicious URL Detector

A lexical-feature-based machine learning classifier that flags likely
phishing/malicious URLs from the URL string alone — no need to fetch the
page, which keeps it safe to run on unverified input.

## How it works

`src/features.py` extracts 13 hand-engineered features from a raw URL
(length, subdomain count, presence of an IP-address host, suspicious
keywords like "login"/"verify", suspicious TLDs, HTTPS usage, etc.), and
`src/train.py` trains a `RandomForestClassifier` on those features.

```bash
pip install -r requirements.txt
python -m src.train
```

Example output:
```
Test accuracy: 1.00
...
Sample predictions on new (unseen) URLs:
  {'url': 'https://www.example.com/products/shoes', 'prediction': 'benign', 'malicious_probability': 0.0}
  {'url': 'http://login-verify-secure.top/paypal/account', 'prediction': 'malicious', 'malicious_probability': 0.96}
```

## Running tests

```bash
pip install pytest
pytest tests/ -v
```

## Project layout

```
malicious-url-detector/
├── src/
│   ├── features.py   # lexical feature extraction
│   └── train.py      # train / evaluate / save the model
├── data/
│   └── sample_urls.py  # small illustrative labeled dataset
├── tests/
│   └── test_features.py
```

## Notes on the dataset

The bundled dataset is a small set of illustrative, synthetic examples so
the repo runs with zero external downloads. For a more rigorous model,
swap in a public dataset such as the UCI "Phishing Websites" dataset or a
PhishTank export, and re-run `python -m src.train`.

## License

MIT — see [LICENSE](LICENSE).
