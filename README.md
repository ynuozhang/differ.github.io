# Differ — enzyme chemistry, in motion

A static, interactive prototype for an enzyme mechanism prediction interface. It accepts substrate SMILES for structure drawing and includes three hand-authored mechanism demonstrations. **The prediction model is not connected.**

The v6 design uses Lora and one continuous warm gradient of rust, brown, red, orange and amber. Five-lobed sugar-maple outlines are drawn directly in SVG. The Reaction workspace and A work in progress sections have clearer outlines and sparse main veins, including on mobile. Restrained amber traces follow the hero contours on a 22-second cycle and the lower-section outlines on a 28-second cycle. Broader matte protein ribbons with fine black contours, thinner connecting loops and a bright point cloud rotate, highlight an illustrative active site, and zoom into an animated reaction drawing as the visitor scrolls. The examples also have a player, a manual progress slider, product SMILES copy buttons and SVG export.

## Preview

Open `index.html` in a modern browser, with the `assets` directory beside it. No install or build step is required. The separately provided `differ-preview.html` embeds its fonts and drawing library for convenient single-file preview; the contour decoration and logo are SVG. Deploy this folder for the smaller, cacheable production version.

## Publish on GitHub Pages

1. Create or open your GitHub repository, for example `differ-site`.
2. Put the **contents** of this folder at the repository root: `index.html`, `.nojekyll`, `README.md`, and the entire `assets/` directory. Do not leave them nested inside an extra `differ-site/` directory if you select the repository root as your source.
3. Commit and push to `main`.
4. Open **Settings → Pages → Build and deployment**. Select **Deploy from a branch**, then `main` and `/(root)`, and save.
5. Use the URL displayed in GitHub's Pages settings after deployment completes. A project site normally uses `https://<account>.github.io/<repository>/`.

All asset URLs are relative (`./assets/...`), so the same package supports a repository subpath, an account-level site and a custom domain. `.nojekyll` disables Jekyll processing. No Node, npm, backend, environment variables or GitHub Actions configuration are required for this branch-based static setup.

GitHub documentation:

- https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
- https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## What works now

- A procedural protein illustration with broader ribbons, flat matte front and reverse colors, fine black edge contours, coiled alpha-helices, thinner connecting loops, a brighter sparse point cloud, drag rotation and active-site hover highlighting. The ribbons retain simple profiles without beta-sheet arrowheads; internal mesh edges remain unstroked.
- Scroll-driven transition into the mechanism, plus direct step navigation.
- Structure drawing from SMILES, with immediate clearing of stale drawings after input edits.
- Three exact demo inputs: chymotrypsin amide hydrolysis, triosephosphate isomerase and NADH-dependent pyruvate reduction by L-lactate dehydrogenase.
- Fixed products and multi-stage curved-arrow drawings for the selected example.
- Product SMILES copying, animation pause/play, native keyboard-accessible progress slider, mobile enlarge-and-pan control, and download of the displayed mechanism frame as SVG.
- Honest pending results for all other drawable inputs. No unrelated reaction is substituted.
- Enzyme context populated from the selected example; it is read-only in this prototype.
- Responsive layouts and reduced-motion support. The hero has a separate motion control. Automatic motion stops when paused, under reduced-motion preferences, or when its scene is offscreen; paused and offscreen players preserve their progress. Sugar-maple outline and vein backgrounds remain visible in the workspace and model-status section at phone width. The hero's decorative contours fade as the active-site zoom begins.
- The example button calls the display handler directly, without depending on native form submission. This supports embedded previews that allow scripts but restrict forms. A molecular-drawing failure does not hide the selected example's mechanism.

SMILES parsing here is a drawing feature, not chemical validation or a prediction. Demo matching trims outer whitespace and otherwise compares the exact supplied SMILES. Equivalent SMILES, different protonation states and different stereoisomers do not automatically count as the same example. Use the example buttons to select the available demonstrations.

## Scientific presentation

These are schematic teaching sequences, adapted from M-CSA, with some elementary events grouped for display. Curved arrows represent electron-pair redistribution. Their pulses and timing do not represent simulated electron trajectories or measured event times.

The hero uses procedurally generated protein ribbons. It is an artistic protein illustration, not a structure reconstructed from experimental coordinates or an atomically faithful reaction environment. It is labeled “Illustrative protein ribbons”; replacing it with a real structure requires an explicit atom/residue mapping.

Sources (M-CSA content is distributed under CC BY 4.0):

- Chymotrypsin: https://www.ebi.ac.uk/thornton-srv/m-csa/entry/387/
- Triosephosphate isomerase: https://www.ebi.ac.uk/thornton-srv/m-csa/entry/324/
- L-lactate dehydrogenase: https://www.ebi.ac.uk/thornton-srv/m-csa/entry/584/
- License: https://creativecommons.org/licenses/by/4.0/

The TIM drawing groups the displayed chemistry into two stages and uses Lys13/His95/Glu165 labeling. LDH is shown in the pyruvate-reduction direction and groups hydride/proton transfer into one illustrative stage; its labels follow the M-CSA reference PDB numbering (His193, Asp166, Arg106, Arg169). The display does not establish whether physical events are concerted or stepwise.

## Connecting a future model

`predictMechanism()` inside `index.html` is the current demo adapter. It resolves `{ mech: matchedExampleOrNull }`; it performs no inference and makes no API calls.

GitHub Pages serves the frontend. A future server-side model needs a separate HTTPS endpoint. Configure that endpoint to accept the actual Pages origin, and keep API credentials on the server.

Before wiring the endpoint, define the real conditioning and response contract. Substrate SMILES alone does not uniquely identify an enzyme-specific reaction; the model may need an enzyme identifier, sequence or structure, cofactors, and other supported context. This prototype should not be treated as a finalized scientific API.

The current renderer consumes an example object like `CHYMO`, `TIM` or `LDH`, including:

- Stable atom IDs and role groups (`sub`, `enz`, `cof`, `wat`).
- Atom positions, bond orders and charges for every depicted state.
- Ordered steps with `from`/`to` states and curved arrows from a lone pair/bond to an atom/bond.
- Product names and SMILES, enzyme information and explanatory copy.

Use a validated adapter from a model response to that rendering structure. Model-provided text must use safe DOM text rendering; several trusted, local mechanism descriptions currently use HTML for chemical notation and must not be replaced with untrusted HTML.

## Included dependencies

Lora upright and italic variable fonts (400–700) and SmilesDrawer 2.1.7 are bundled locally, together with the hawk mark and favicon as SVG assets. The decorative sugar-maple outlines and vein lines are inline SVG in `index.html`; no botanical raster artwork is required or loaded. The site does not need Google Fonts or a CDN at runtime. The chemical input and SMILES strings use the system monospace font for symbol clarity; all other interface typography uses Lora. The standalone preview embeds the dependencies it needs.

See `assets/THIRD_PARTY_NOTICES.txt` and the included font/software licenses. Downloaded SVGs embed drawing styles and graphical definitions; their text specifies Lora with a serif fallback rather than embedding the font binaries.

## Visual direction

The entire page shares a continuous warm gradient, including the workspace, result panels and footer. Brown and rust surfaces, red and orange accents, amber light and warm cream text replace the earlier green hero and pale page sections. There is no automatic theme switch, and green is not part of the interface palette.

The supplied photographs inform the warm colors. V6 keeps the visible linework and gives the protein broader, clearly outlined ribbons:

- Recognizable five-lobed sugar-maple outlines include branching vein lines. Oversized leaves are cropped at the page edges while keeping enough of each shape visible to read as a leaf.
- The linework appears in the hero, Reaction workspace and A work in progress sections. Its placement and contrast are adapted for both desktop and mobile, including the space around the workspace controls and the model-status copy.
- An amber trace follows the linework on a slow, 22-second cycle. It stops when motion is paused, reduced motion is requested, or its section is offscreen.
- The hero contours fade during the zoom into the active site, leaving the reaction drawing as the focus. The lower sections keep their own visible maple backgrounds.
- Broader protein ribbons use flat matte colors with darker reverse faces and fine black contours along their edges. Thinner loops connect the ribbon elements, and internal tessellation edges remain unstroked. Sheet arrowheads and shiny surface highlights remain absent. The brighter warm point cloud stays visible around the ribbons.
- The banking hawk keeps its naturally rounded head, compact hooked beak and pointed wing and tail feathers. It appears beside the Lora wordmark and in the favicon.

There are no falling leaves, orbital rings or botanical raster illustrations. Protein rotation, the active-site zoom, electron-pair arrows and the single restrained contour trace provide the motion. Full-size reference photographs are not loaded at runtime.

## Review changes

- Replaced Ubuntu with locally bundled Lora and unified every section under one continuous warm gradient and dark color system.
- Replaced botanical raster artwork with cropped, five-lobed sugar-maple outlines and vein lines; made the backgrounds visible in both lower sections, including mobile, and retained the slow amber trace.
- Broadened the matte protein ribbons and added fine black edge contours, with thinner connecting loops and unstroked internal mesh edges. The ribbons keep simple profiles without sheet arrowheads, and the point cloud remains bright.
- Retained the banking-hawk silhouette with its rounded head and sharp feather tips.
- Refined the headline, navigation, spacing, subtle texture and small-screen presentation.
- Reduced the default scroll story from 1125vh to 800vh and retained direct access to the workspace.
- Replaced unsupported prediction/training claims with prototype and development language.
- Removed unrelated-substrate fallback, unsafe stereochemical aliases and the ignored editable enzyme field.
- Parse the current submission directly; clear stale molecules and results; guard interrupted/duplicate submissions; recover the busy state after errors.
- Handle the example button through a direct click listener and preserve mechanism access when optional molecular drawing fails.
- Keep the hero unchanged when a result is selected, avoiding a large page-height jump above the workspace.
- Freeze progress and particle time on pause, preserve progress offscreen, and add a scrubber and drawing export.
- Use explicit SVG mask bounds so single bonds remain visible for small molecules.
- Add nearby schematic labeling, M-CSA source links, safe external-link attributes, metadata, favicon, skip link and status announcements.

## Logo assets

`assets/hawk-mark.svg` is a scalable, single-color silhouette on a transparent background. `assets/favicon.svg` places the same mark on a warm brown tile. The header embeds the SVG path so its fill follows the navigation text color.

The design is a banking hawk tilted toward the upper-left corner, with a naturally rounded head, compact hooked beak, pointed primary feathers and pointed tail feathers. Image-generated studies informed the direction; the production mark is a manually authored vector with no raster fringe.

## Verification

Earlier revisions were inspected in the managed browser at desktop width and in a 390 × 844 CSS viewport. The review exercised all three examples, unknown-input pending behavior, invalid input followed by immediate valid submission, manual progress control, pause, scroll-scene navigation and local asset paths. These are prototype checks, not validation of a predictive model or exhaustive cross-browser testing.

The earlier cartoon revision was additionally checked on all three enzyme scenes, through the active-site zoom and at phone width. The falling-leaf canvas and drawing code, and the orbital rings, have been removed.

Before the v4 visual update, the standalone preview was checked in an iframe with `sandbox="allow-scripts"` and no `allow-forms`: its direct button displayed the example products and four-stage mechanism, then scrolled to and focused the results. The production folder also loaded its fonts and other assets from a nested `/github-pages-check/` path. Desktop and 390 × 844 layouts showed no horizontal page overflow, and the mobile button displayed the expected results.

The v4 design was inspected at desktop width and in the same 390 × 844 phone layout. All three illustrative folds were reviewed with the rounded surface renderer; drag rotation and the active-site transition were exercised. The contour layer faded to zero opacity during the active-site view and its trace paused. Both layouts had no horizontal page overflow and used Lora. The v4 standalone preview also opened the example products and four-stage mechanism inside the restricted iframe described above. Rendering timings were inspected with a temporary development overlay; it is not included in this package, and the remote browser is not a benchmark for real devices. JavaScript remains required for the interactive features.

The v5 design was reviewed in the managed browser at desktop width and in a 390 × 844 phone viewport. Sugar-maple outlines and main veins were visibly present in both lower sections, with no horizontal page overflow. The default hero rendered narrow matte ribbons without sheet arrowheads. Both lower-section traces paused with the existing motion control. The example button displayed the chymotrypsin products and four mechanism stages. All three ribbon folds also passed finite-geometry checks at desktop and phone detail, with 18 native-Canvas draws covering the initial, zoom-transition and active-site views. These are prototype checks, not an exhaustive browser or device test.

The v6 renderer was inspected in the managed browser at desktop width and in the 390 × 844 phone viewport. Broader flat ribbon faces and black outlines were visible, with no internal wireframe pattern. Manual drag rotation remained responsive while automatic motion was paused. The active-site view cleared the ribbon surfaces and their contours; the mechanism controls still opened the example products and stages. The TIM example rebuilt the hero with the same outlined helix treatment. The phone page had no horizontal overflow. All three folds also passed finite-geometry and edge-adjacency checks, with 18 native-Canvas draws covering desktop and phone detail at the initial, intermediate-zoom and active-site views. These are focused prototype checks, not exhaustive browser or device testing.
