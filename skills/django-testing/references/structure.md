# Test package structure

Mirror production modules:

```
project_name/
├── app_name/
│   ├── services/
│   │   └── payments.py
│   └── tests/
│       ├── factories.py
│       └── services/
│           └── test_payments.py   # or test_item_buy.py per function
└── common/
    ├── utils/
    │   └── files.py
    └── tests/
        └── utils/
            └── test_files.py
```

Keep factories close to the app (`tests/factories.py`) unless shared across apps — then a `common/tests/factories` package is fine.
