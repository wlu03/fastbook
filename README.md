fastbook/
├── pyproject.toml
├── setup.py
├── Makefile
├── README.md
├── LICENSE
├── .gitignore
│
├── src/
│   ├── parser.h                  # parse_bba() signature, BBAResult struct
│   ├── parser.cpp                # specialized key scanner, field extraction
│   ├── float_fast.h              # pow10 lookup table, decimal→double
│   ├── float_fast.cpp            # bounds-checked fast float impl
│   ├── _parser.pyx               # cython wrapper: borrow UTF-8, release GIL, return tuple
│   └── _parser.pxd               # cython declarations for C++ symbols
│
├── fastbook/
│   ├── __init__.py               # exports parse(), FeedClient
│   ├── types.py                  # BBA namedtuple / dataclass
│   ├── fallback.py               # orjson fallback when native returns None
│   ├── feed.py                   # async websocket ingestion loop
│   ├── schemas.py                # per-exchange key names + expected field counts
│   └── platform/
│       ├── __init__.py
│       ├── base.py               # abstract ExchangeAdapter interface
│       ├── binance.py            # bookTicker WS, subscribe msg, field map
│       ├── polymarket.py         # market channel WS, asset_id subscription
│       └── kalshi.py             # authenticated WS, orderbook_delta channel
│
├── tests/
│   ├── conftest.py               # fixtures: sample JSON msgs, mock WS server
│   ├── test_parser.py            # known inputs → expected tuples
│   ├── test_adversarial.py       # schema drift, false key matches, unicode escapes
│   ├── test_fallback.py          # verifies orjson triggers on parse failure
│   ├── test_feed.py              # mock WS, end-to-end message flow
│   └── test_exchanges.py         # per-adapter subscribe/field-map tests
│
├── bench/
│   ├── bench_parse.py            # pyperf: native vs orjson on captured msgs
│   ├── bench_e2e.py              # full pipeline: WS recv → parse → emit
│   └── captured/
│       ├── binance_bookticker.json
│       ├── polymarket_market.json
│       └── kalshi_orderbook.json
│
└── examples/
    ├── binance_stream.py         # connect + print BBAs
    ├── polymarket_spread.py      # stream spreads in real time
    └── kalshi_orderbook.py       # stream orderbook deltas