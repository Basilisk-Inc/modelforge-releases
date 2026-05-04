# ModelForge End User License Agreement

**Version 1.0 — Effective May 2026**

> ⚖️ This is a proprietary commercial license, not an open-source license. The source code is not distributed; the binaries you download are licensed for use under the terms below. If you need a different arrangement (research, redistribution, embedding in another product), [contact us](mailto:TODO_LEGAL_EMAIL).

---

## 1. Definitions

- **"Software"** — the ModelForge binary executable, bundled Python runtime, web UI assets, documentation, and any updates Licensor provides.
- **"Licensor"** — TODO_LEGAL_ENTITY_NAME, the publisher of ModelForge.
- **"Licensee"** ("you", "your") — the individual or legal entity that downloads, installs, or uses the Software.
- **"GPU"** — a single physical or virtual graphics processing unit, identified by its hardware UUID, attached to a Machine.
- **"Machine"** — a single physical or virtual computer system, identified by ModelForge's machine fingerprint.
- **"Tier"** — Community, Pro, or Enterprise, as documented in ModelForge's pricing.
- **"License File"** — an Ed25519-signed token issued by Licensor authorizing a Tier on a specific Machine.

## 2. Grant of License

Subject to your compliance with this Agreement, Licensor grants you a non-exclusive, non-transferable, revocable license to install and use the Software on Machines you own or control, for your internal business or personal purposes, limited by the Tier you have activated.

The first GPU on any Machine running ModelForge is automatically licensed at the **Community Tier** at no cost. Additional GPUs require a paid License File for that Machine.

## 3. Restrictions

You may not, and may not permit anyone else to:

a. **Reverse-engineer.** Disassemble, decompile, or otherwise attempt to derive source code, algorithms, or trade secrets from the Software, except to the limited extent applicable law expressly permits despite this prohibition.

b. **Tamper with licensing.** Modify, bypass, defeat, or circumvent the license-enforcement mechanisms (machine fingerprinting, GPU UUID checks, signature verification, anti-rollback timestamps).

c. **Redistribute.** Sell, rent, lease, sublicense, distribute, or transfer the Software to any third party, except as part of an authorized internal transfer of the Machine on which it is installed.

d. **Multi-party use under a Community Tier.** Use a Community Tier installation as part of a service offered to third parties (e.g. a public API, a hosted SaaS feature). Pro or Enterprise Tier required.

e. **Remove notices.** Remove, alter, or obscure copyright, trademark, or proprietary notices in the Software or its documentation.

## 4. License Tiers

The Tier active on a Machine determines the maximum number of GPUs ModelForge will use on that Machine:

| Tier | GPUs per Machine | Activation |
|---|---|---|
| Community | 1 | Automatic — no License File required |
| Pro | up to 4 | Paid License File issued by Licensor |
| Enterprise | unlimited | Negotiated commercial agreement |

Tier feature differences, pricing, and minimum commitments are published on Licensor's pricing page and may be updated with reasonable notice.

GPUs in excess of the licensed count present on the Machine will be unused by the Software but do not constitute a breach.

## 5. License Files

Each paid License File is bound to:

- A specific Machine, identified by a fuzzy 2-of-3 fingerprint hash of CPU, motherboard, and primary network adapter identifiers.
- A specific GPU count and Tier.
- An issuance date and an expiration date.

A License File may be re-issued at no charge if **at most one** of the three fingerprinted hardware components has been replaced. Replacing two or more components requires a new purchase or, for Enterprise customers under contract, follows the procedure in the agreement.

## 6. Updates and Releases

Licensor may release new versions of the Software. Your existing License File continues to authorize new versions released within its validity period. Versions released after your License File expires require a renewal.

Updates are not retroactively delivered — installing a new version is an action you take, not something the Software does automatically.

## 7. Data and Telemetry

The Software does not transmit Licensee data to Licensor or any third party as part of normal operation. License-server requests during online activation transmit only:

- The Machine fingerprint hash
- The Tier and GPU count being requested
- The product key supplied by Licensee

No training data, conversation contents, model weights, datasets, or filesystem contents are ever transmitted.

Offline activation transmits nothing — the fingerprint file is delivered out-of-band by Licensee and a License File is returned out-of-band by Licensor.

## 8. Third-Party Components

The Software incorporates third-party open-source software, listed in the `THIRD_PARTY_LICENSES.txt` file shipped with each release. Each such component is governed by its own license; nothing in this Agreement modifies your rights or obligations under those licenses.

## 9. Intellectual Property

The Software is licensed, not sold. Licensor retains all right, title, and interest in and to the Software, including all intellectual property rights. No rights are granted to you except as expressly set forth in this Agreement.

Models you fine-tune using the Software are yours. Datasets you upload are yours. Adapters produced by training runs are yours. Licensor claims no ownership over any of these.

## 10. Disclaimer of Warranty

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NONINFRINGEMENT. IN NO EVENT SHALL LICENSOR BE LIABLE FOR ANY CLAIM, DAMAGES, OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT, OR OTHERWISE, ARISING FROM, OUT OF, OR IN CONNECTION WITH THE SOFTWARE OR ITS USE.

## 11. Limitation of Liability

TO THE MAXIMUM EXTENT PERMITTED BY APPLICABLE LAW, LICENSOR'S TOTAL CUMULATIVE LIABILITY ARISING OUT OF OR RELATING TO THIS AGREEMENT SHALL NOT EXCEED THE FEES YOU PAID LICENSOR FOR THE LICENSE TO THE SOFTWARE IN THE TWELVE (12) MONTHS PRECEDING THE EVENT GIVING RISE TO LIABILITY. IN NO EVENT SHALL LICENSOR BE LIABLE FOR INDIRECT, INCIDENTAL, SPECIAL, CONSEQUENTIAL, OR PUNITIVE DAMAGES, INCLUDING LOST PROFITS, LOST DATA, OR BUSINESS INTERRUPTION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.

The Community Tier is provided free of charge; no monetary remedy is available for issues arising from Community Tier use.

## 12. Termination

This Agreement is effective until terminated. Your rights under this Agreement terminate automatically without notice if you fail to comply with any term. On termination, you must cease all use of the Software and remove all copies from your Machines.

Termination of paid Tiers for non-payment follows the procedure in the applicable order form or commercial agreement.

## 13. Export Compliance

You may not use, export, or re-export the Software except as authorized by the laws of the jurisdiction in which the Software was obtained and applicable export-control laws, including but not limited to those of the European Union, the United States, and any country to which you are exporting.

## 14. Governing Law and Disputes

This Agreement is governed by the laws of Portugal, without regard to its conflict-of-laws principles. Any dispute arising out of or relating to this Agreement shall be resolved exclusively in the courts of Lisbon, Portugal, and you consent to personal jurisdiction in those courts.

## 15. Entire Agreement

This Agreement, together with any order form or commercial agreement entered separately for paid Tiers, constitutes the entire agreement between you and Licensor regarding the Software and supersedes all prior or contemporaneous communications and proposals.

---

For commercial licensing, custom terms, or questions about this Agreement, contact: TODO_LEGAL_EMAIL.