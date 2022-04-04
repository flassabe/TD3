# TD3: Fingerprinting

In this session, you will implement fingerprinting algorithms. Matching will follow three solutions:

- Simple matching
- Histogram matching
- Gauss matching

## Simple matching

Prior to simple matching, a new database whose values are converted from `RSSISample` to `float` shall be built. The new float value is an average value corresponding to all RSSI in the sample.

The, you shall implement the following function, which computes for a device RSSI vector (defined by a dictionary whose keys are references MAC addresses and whose value is the floating point value of the average RSSI) the distance in RSSI space with all fingerprints and returns the fingerprint's location which distance is the shortest.

```python
def rssi_distance(sample1: dict[str, float], sample2: dict[str, float]) -> float:
	# Your code
	pass

def simple_matching(db: FingerprintDatabase, sample: dict[str, float]) -> Location:
	# Your code (use rssi_distance)
	pass
```

## Histogram matching

Histogram matching relies on modifying the DB as defined in TD1, as follows: each RSSISample shall be normalized. Normalization consists in making the sum of all beans of each histogram equal to 1, i.e. each occurrence count is replaced by its value divided by the total occurrences count for the sample. For instance:

```python
rssi = {"aa:bb:cc:dd:ee:ff": 3, "ff:ee:dd:cc:bb:aa": 1}
```

shall be converted to:

```python
rssi = {"aa:bb:cc:dd:ee:ff": 0.75, "ff:ee:dd:cc:bb:aa": 0.25}
```

Of course, device samples also shall be normalized. `RSSISample` becomes `NormHisto`:

```python
class NormHisto:
	def __init__(self, histo: dict[int, float]):
		self.histogram = histo
```

We use the following definition to compute similarities between two histograms: similarity is a probability computed by the overlapping parts of both histograms, i.e. it is equal to the sum of the minimum value for all common RSSI values. See [here](https://www.youtube.com/watch?v=MW9qRgAtiX0) for more details.

This matching considers the most probable fingerprint as the device location.

```python
def probability(histo1: NormHisto, histo2: NormHisto) -> float:
	# Your code
	pass

def histogram_matching(db: FingerprintDatabase, sample: NormHisto) -> Location:
	# Your code (use probability)
	pass
```

## Gauss matching

Before proceeding to Gauss matching, you shall build a new fingerprint database whose `RSSISample` are swapped with `GaussModel` defined as:

```python
class GaussModel:
	def __init__(self, avg: float, stddev: float):
		self.average_rssi = avg
		self.standard_deviation = stddev
```

For gauss matching, we will use histogram matching after transforming Gauss definition as average RSSI and standard deviation to histogram values. You shall compute histogram values for all integer values of RSSI from floor(rssi_avg)-10 to floor(rssi_avg)+10. You may store computed histograms to avoid calculation on each request.

```python
def histogram_from_gauss(sample: GaussModel) -> RSSISample:
	# Your code
	pass
```
