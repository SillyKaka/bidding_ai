# bidding_ai

# bidding_ai

Bridge bidding assistant (2/1, strong 1NT 15–17) focused on IMP EV recommendations.

## v0.1 Scope
- You provide: your hand (South/North/East/West), vulnerability, dealer, and bidding history.
- The service returns:
  - recommended next call + a few alternatives
  - explanation (2/1 terms)
  - follow-up plan (common partner responses branches)
  - EV(IMP) placeholders (evaluation engine skeleton included; DDS integration planned)

System settings (v0.1 defaults):
- System: 2/1
- 1NT opening: 15–17
- Transfers: NO
- Jacoby 2NT: NO
- Weak twos: NO

## Quickstart (Docker)
```bash
docker build -t bidding-ai .
docker run --rm -p 8000:8000 bidding-ai
```

Then open:
- http://localhost:8000/docs

## Quickstart (local)
```bash
python -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
uvicorn bidding_ai.api.main:app --reload
```

## Example request
```bash
curl -s http://localhost:8000/recommend \
  -H 'content-type: application/json' \
  -d '{
    "seat": "S",
    "dealer": "N",
    "vul": "NONE",
    "auction": ["P","P"],
    "hand_pbn": "AKQ73.J4.Q92.83"
  }' | jq
```

## Notes
- `hand_pbn` uses PBN order: `S.H.D.C` with ranks `AKQJT98765432` (no separators).
- In v0.1 the evaluator is intentionally simple; EV numbers are not yet meaningful until DDS (or another trick estimator) is connected.
