# ParkMatch

ParkMatch helps people pick which U.S. National Park System sites to visit. You choose what
matters to you (states, activities, facilities, a highest entrance fee, whether you need a
campground) and it lists the parks that fit, with a short reason for each one. You can also
look up one park and compare a few side by side.

This is our group project for CSE 583, Software Development for Data Scientists (University of
Washington, Autumn 2026).

## Who it is for

People planning a trip who don't know the park system well, such as first-time visitors,
families, campers and people who need accessible facilities. They use websites and map apps,
not Python. For now the package is used from Python; a map and a web page are planned.

## Data

We join two public sources from the National Park Service on the park code:

- The [NPS API](https://www.nps.gov/subjects/developer/api-documentation.htm): park details,
  activities, facilities, campgrounds and fees, one row per park.
- The NPS Visitor Use Statistics ([doi:10.57830/2317666](https://doi.org/10.57830/2317666)):
  recreation visits per park and month, which we add up by year.

A copy of both, downloaded on 2026-11-05, ships with the package, so you don't need an API key.
[src/parkmatch/data/README.md](src/parkmatch/data/README.md) explains how to refresh it.

## Install

Python 3.11 or newer.

```
git clone https://github.com/EstelleJJJJJ/ParkMatch.git
cd ParkMatch
pip install -e ".[dev]"
pytest
```

## Example

```python
import parkmatch

table = parkmatch.build_park_table()
hits = parkmatch.find_parks(table, states=["WA"], activities=["Hiking", "Camping"], max_fee=35)
print(hits[["full_name", "match_score", "match_reasons"]].head(3).to_string(index=False))
```

```
                   full_name  match_score                             match_reasons
       Olympic National Park          1.0 Has Hiking and Camping; entrance fee $15.
 Mount Rainier National Park          1.0 Has Hiking and Camping; entrance fee $15.
North Cascades National Park          1.0    Has Hiking and Camping; free entrance.
```

There's a longer example in [examples/find_parks_example.py](examples/find_parks_example.py).

## What's in the repository

```
design/              user stories and use cases
docs/                functional and component specifications
src/parkmatch/       the package: data.py loads and joins the data, matcher.py searches and compares
test/                tests, run with pytest
examples/            an example script
scripts/             build_data.py, only used to refresh the data
```

GitHub Actions runs `ruff check .` and `pytest` on every push and pull request.

## Contributors

| Name | GitHub | So far | Looks after from now on |
|---|---|---|---|
| Yucheng Jiang | @EstelleJJJJJ | repository setup, user story template, two more user stories | [UC4](https://github.com/EstelleJJJJJ/ParkMatch/issues/11): see parks on a map |
| Peiyan Xie | @peiyax | the first 30 user stories, removed the extra user_cases.md | [UC2](https://github.com/EstelleJJJJJ/ParkMatch/issues/15): look up one park |
| Zitong Ren | @ari11a | src folder in the class git exercise, removed duplicate user stories | [UC3](https://github.com/EstelleJJJJJ/ParkMatch/issues/12): compare parks side by side |
| Yuxia Liang | @zl1212-ship-it | use cases, specifications, first version of the package | [UC1](https://github.com/EstelleJJJJJ/ParkMatch/issues/14): find parks that match what I like |

From now on each of us looks after one use case: its issue, its code and tests, and its part
of the docs. We each present our own use case in our talks, look after the shared parts (the
data, the web page and CI) together, and review each other's pull requests.

## License

MIT, see [LICENSE](LICENSE).
