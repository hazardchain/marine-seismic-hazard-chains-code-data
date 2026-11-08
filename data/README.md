# Data — documented seismic hazard-chain records

Input tables for the network analysis. Each is a Word table; the code reads the
event `ID` (first column) and the `Proposed Encoding` column.

| Column | Meaning |
|--------|---------|
| `ID` | Source event of the cascade (e.g. `LTT_Honshu_2024`, `1994-0307-CHN`). Rows without an ID continue the event above. |
| `Observed Cascading Effects` | Human-readable chain (e.g. `SEQ > Tsunami > Coastal Flooding > Death`). |
| `Proposed Encoding` | Machine-readable directed links of that chain, `trigger,target;trigger,target;…` (a `+` branch becomes two links). |

## Inventory

### `marine/`
- `tsunami_Dohmen_2025_EQ.docx` — the marine record behind the paper: 366
  documented chains under 58 source events (57 events of the landslide-tsunami
  catalogue of Dohmen et al., 2025, and the 2011 Tohoku tsunami). It yields the
  39-node / 152-edge network, every Table 2 degree and the Table 3 layer net flow
  (+42 / +1 / −16 / −27).
- `tsunami_extra_events_2004_1960_2018_EQ.docx` — the three independent events
  of Sect. 3.5 (2004 Indian Ocean, 1960 Valdivia, 2018 Palu), with a `Source`
  column giving the provenance of every link. The 2018 Palu event is listed in
  the catalogue but was withheld from the compilation for this test.

### `mainland_china/`
Earthquake hazard-chain records for China, split by region (94 events, 605
chains), from EM-DAT and the China Earthquake Networks Center:
- `earthquake_table_China_4_E.docx` — East China (including Taiwan)
- `earthquake_table_China_4_N.docx` — North China
- `earthquake_table_China_4_NW.docx` — Northwest China
- `earthquake_table_China_4_SW.docx` — Southwest China

The comparison network of the paper (Sect. 2.1 and 3.3) uses the **inland**
earthquakes only: epicentre on land and no tsunami in the chain. Ten events with
offshore epicentres or near-coast cascades (nine Taiwan events and the 1994
Taiwan Strait event) are excluded in the code (`inland_excluded`), which leaves
84 events, 500 chains, 14 classes and 44 edges. The files themselves are kept
complete.

### `Aggregation of the 39 marine hazard nodes into the 15 common.docx`
The 39 → 15 class mapping used for the marine–inland comparison (paper appendix).

## Edge-construction criterion
A directed edge `i→j` is placed in the network if and only if the ordered peril
pair (i, j) is documented in the `Proposed Encoding` of at least one recorded
cascade chain; the union over all chains is then de-duplicated (self-loops
retained in the fine marine network, removed for the 15-class comparison). The
analysed graph is thus binary: it records which couplings are documented, not
how often they occur.
