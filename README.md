# CP User API

A Flask-based REST API that fetches and returns competitive programming user profile data from multiple platforms.

---

## Supported Platforms

The API currently supports **3 platforms**:

| # | Platform     | Endpoint Segment | Data Provided |
|---|-------------|-----------------|---------------|
| 1 | **Codeforces** | `/api/codeforces/<username>`   | Handle, rating, rank, contest history (name, rank, rating change) |
| 2 | **LeetCode**   | `/api/leetcode/<username>`     | Ranking, problems submitted/solved, acceptance rates, contribution points, reputation |
| 3 | **AtCoder**    | `/api/atcoder/<username>`      | Rating, highest rating, rank, level, country, birth year, affiliation |

---

## API Usage

### Endpoint

```
GET /api/<platform>/<username>
```

### Examples

**Codeforces**
```
GET /api/codeforces/tourist
```

**LeetCode**
```
GET /api/leetcode/leetcode
```

**AtCoder**
```
GET /api/atcoder/kAmi
```

### Successful Response (`status: "OK"`)

```json
{
  "status": "OK",
  "rating": 1500,
  "contests": [
    {
      "contest": "Example Contest One",
      "rank": "42",
      "solved": "NA",
      "ratingChange": "+100",
      "newRating": "1600"
    }
  ]
}
```
*(Fields vary by platform — see Supported Platforms table above.)*

## Installation & Setup

### Prerequisites

- Python 3.x
- pip

### Install Dependencies

```bash
pip install -r requirements.txt
```

**Dependencies:**
- regex
- requests-html
- beautifulsoup4
- Flask
- Flask-RESTful
- Flask-CORS
- gunicorn
- lxml_html_clean

### Run Locally

```bash
python app.py
```

The server runs on `http://127.0.0.1:5000` by default.

### Deploy (Heroku / similar)

The project includes a `Procfile` for Heroku-style deployments:

```
web: gunicorn app:app
```

---

## Running Tests

```bash
pytest tests/
```

The test suite includes characterization tests for the 3 supported platforms, using fixture-backed responses to avoid hitting upstream APIs.

---

## Project Structure

```
.
├── app.py                  # Flask app & API routing
├── util.py                 # Core logic: platform fetchers & data parsing
├── requirements.txt        # Python dependencies
├── Procfile                # Deployment config
├── README.md               # This file
├── tests/
│   ├── test_characterization.py  # API & platform tests
│   └── fixtures/               # HTML/JSON response fixtures per platform
│       ├── codeforces/
│       ├── leetcode/
│       └── atcoder/
```

---

## API Design Notes

- **Platform dispatch**: The `UserData` class in `util.py` routes each platform request to a private method (`__codeforces`, `__leetcode`, `__atcoder`). Unsupported platforms raise a `PlatformError`.
- **Error handling**: All exceptions are caught in `app.py`'s `Details` resource and mapped to friendly JSON failure responses.
- **Diagnostics**: Detailed diagnostic logs are emitted for every upstream request and parsing stage via `_log_diagnostic`.

---

## License

MIT
