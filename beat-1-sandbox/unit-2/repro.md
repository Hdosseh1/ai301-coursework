Reproduced on current `main`. Details below.

**Environment**

- macOS 15.5, Apple Silicon (arm64)
- Python 3.11.13
- Commit `99673c7` (`main`)
- Dependencies installed with `pip install -e ".[dev]"` in a `.venv`. I didn't start the Docker services or run the migrations, since the scrubber doesn't use them.

**Steps**

From the repo root with the venv activated, I ran the example from the issue:

```
$ python - <<'EOF'
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
EOF
Call me at (555) 123-4567 or [REDACTED]
2026-10-06 00:36:06 [info     ] pii_detected                   count=0 types=0
[]
```

Control, the dashed format on its own:

```
$ python -c "from safety.pii_scrubber import PIIScrubber; print(PIIScrubber().detect('Call me at 555-123-4567'))"
2026-10-06 00:36:06 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

Then the scrubber's tests:

```
$ pytest tests/unit/test_pii_scrubber.py -q -rx
..xx.......x.....x....x..                                                [100%]
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
20 passed, 5 xfailed in 0.21s
```

The four tests named in the issue are marked `xfail` for #53, and so is a fifth one, `test_mixed_pii_and_text`, which the issue doesn't list.

**Expected:** `(555) 123-4567` is redacted by `scrub()` and reported by `detect()`, the same as `555-123-4567`.

**Actual:** `(555) 123-4567` passes through `scrub()` unchanged and `detect()` returns `[]` (`count=0`), while the dashed number in the same string is redacted and detected as `phone_us`.
