# orca-call-detector
Neural Network integrated into a website that detects endangered Southern Resident Orca calls from underwater hydrophone recordings. 

Submission for the Congressional App Challenge 2026 Competition -- Washington District 8 -- Gabriel Eni

# Mission Statement
With lower fish populations–specifically salmon- other species are affected as well, with Orcas being one of them. It’s hard to combat climate change, but we can help keep track of Orcas, understand their behavior, and understand our impacts on them through their behavior–movement and calls that can signal new baby orcas and communication between groups. I’ve been fishing in the Puget Sound since I was five, so making sure we can keep track of our ecosystems is super important and personal to me.

# How the App Helps
According to the Center for Whale Research’s 2026 census, there are only 76 Southern Residents left . One threat is noise: they hunt salmon by echolocation, and boat engines drown it out. Underwater microphones called hydrophones can hear whales day and night, but they record more audio than any human can listen to.
So OrcaDetector listens first. It cuts a recording into 2.45-second windows and turns each one into a spectrogram, a kind of “sound picture”, with pitch on the y-axis and time as the x-axis. Orca calls show up as bright streaks and curves. I built a convolutional neural nettwork built in TensorFlow that scans that picture. I trained it on real recordings from the OrcaSound hydrophone network, labeled by people, and then tested it on other recordings not used in the training dataset.


# Results
| Version | Training data | AUC   | Recall | Precision | Threshold | <br>
| v0.1    | Round 2       | 0.705 | 16%    | 97%       | 0.50      |<br>
| v0.2    | Round 2       | 0.852 | 74%    | 84%       | 0.20      |<br>
| v0.3    | Rounds 2 + 3  | 0.881 | 87.9%  | 74%       | 0.20      |

All versions are measured on the same benchmark: 388 windows from seven Round 2
recordings that were never used for training.

**v0.1 -> v0.2:** v0.1 underfit because each window was scaled to its own
  loudest point, which erased absolute loudness. v0.2 keeps loudness and
  standardizes each frequency band using the training data.
  
**v0.2 ->v0.3:** added a second day of recordings (Round 3).
Each setup was trained three times:

(AUC --Area Under Curve -- comparison) <br>
| Training data | Run 1 | Run 2 | Run 3 | Average |<br>
| Round 2 only  | 0.852 | 0.794 | 0.840 | 0.829   |<br>
| Rounds 2 + 3  | 0.829 | 0.871 | 0.881 | 0.861   |<br>

v0.3 is the best of the three Rounds 2 + 3 runs, and the threshold of 0.2 was tested to provide a decent trade-off of precision for recall. These figures are somewhat optimistic.

# Limitations
Trained on two days of audio (July 5, 2019 and September 27, 2017), both
from one hydrophone (Orcasound Lab).
Untested at other locations, seasons, and noise conditions.
Not yet evaluated on Orcasound's separate test set.
Detects calls only; does not identify pods yet.

# Use of AI tools disclosure

The machine-learning portion of this project was built by scratch by hand, but I used Claude to help develop the website, as I'm not as familiar with CSS, HTML, and Javascript as the Python coding language. Claude provided an example website which I then modified.

# Data and credits
Audio and labels: [Orcasound](https://www.orcasound.net) hydrophone network, Pod.Cast annotation system, licensed [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). This project is non-commercial and shares alike under the same terms.

Population count (76): Center for Whale Research, July 2026 census.
