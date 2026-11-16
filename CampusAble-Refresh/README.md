# CampusAble

A campus explorer for George Mason University, built on the existing [campusable-week-5 Angular project](https://github.com/szelawa/campusable-week-5). Original project: IT 390, Sufyan Zelawar.

The refresh keeps Angular, the original four Fairfax building descriptions, and their official source links. It adds a new layout, interactive maps, campus switching, search, bookmarks, display preferences, and clearer building details.

## Run on your computer

Use Node.js 24 and npm. Extract the project, open its folder in a terminal, then run:

```bash
npm ci
npm start
```

Open **http://localhost:4200**. Keep the terminal open while using the app. Stop it with **Control+C**. After installing once, use `npm start` for later visits.

Do not open `src/index.html` directly. Angular needs to compile the source first.

## What works

- Switch between Fairfax, Arlington / Mason Square, and Manassas / SciTech.
- Browse ten building records with approximate map locations from OpenStreetMap.
- Search by building name, abbreviation, or features, and filter by category.
- Select a place on the map or from the list, then view its description and official information source.
- Save and unsave places. Bookmarks remain in this browser between visits and are grouped by campus.
- Switch to list view, including when map tiles are unavailable.
- Adjust contrast, text size, and motion. Display settings are saved in this browser.
- Choose a starting building or request your device location, then open walking directions in Google Maps.
- Reach official Mason accessibility, transportation, campus map, and barrier-reporting resources.

## Accessibility data and limitations

Building pins are approximate building centers, **not verified accessible entrances**. This project does not have verified entrance, elevator, restroom, slope, or step-free route data. It also does not receive live construction closures or elevator outages. Google Maps walking directions do not guarantee a step-free route.

Mason's [campus maps](https://www.gmu.edu/campus-maps) and [accessible travel resources](https://accessibility.gmu.edu/campus-access/accessible-travel-routes-resources/) are linked in the app. People who need a verified accessible route should check these resources or contact Disability Services.

This is a curated set of buildings, not a complete campus directory. No sample closure alerts or fictional accessibility claims are shown.

## How the code is organized

| File                         | Purpose                                                                                                                                                    |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `src/main.ts`                | Starts the Angular application.                                                                                                                            |
| `src/app/app.component.ts`   | Holds the selected campus, search, bookmarks, preferences, map interactions, and dialog behavior.                                                          |
| `src/app/app.component.html` | Defines the navigation, place cards, map controls, settings, resources, and dialogs.                                                                       |
| `src/app/campus-data.ts`     | Contains campus centers and building information. Add new building records here. The original Google Maps links and embeds remain in the original records. |
| `src/app/campus-utils.ts`    | Handles search filtering, saved-ID validation, and Google Maps direction URLs.                                                                             |
| `src/app/icon.component.ts`  | Reusable local SVG icons; no icon service is required.                                                                                                     |
| `src/styles.css`             | The visual theme, responsive layout, focus styles, contrast setting, and text-size setting.                                                                |
| `src/assets/`                | Locally served DM Sans variable font and its license.                                                                                                      |
| `tests/campus.test.mjs`      | Tests campus data, search, filters, bookmarks, and direction URLs.                                                                                         |
| `angular.json`               | Angular build settings. Production files are written to `dist/`.                                                                                           |

The app uses Angular signals, as the original did. `filteredBuildings` combines the selected campus, search, category, and bookmark state. Leaflet renders the map from those records. Each marker and list item selects the same building record.

## Maps and privacy

Leaflet is bundled with the app. OpenStreetMap supplies map tiles without an API key; an internet connection is required to load them. Keep the OpenStreetMap attribution visible and review the [tile usage policy](https://operations.osmfoundation.org/policies/tiles/) before a larger public deployment. A production app with significant traffic should use an appropriate tile provider.

Location is requested only after choosing **Find my location** or **My current location**. The app does not store coordinates. If you choose device location as a directions origin and open Google Maps, those coordinates are included in that link. HTTPS or localhost is required for browser geolocation.

Bookmarks and display preferences use localStorage on the current browser. They are not an account and do not sync to other devices. If browser storage is blocked, changes work for the current visit and a message explains that they could not be saved.

## Build and verify

```bash
npm test
npm run build
```

The static production files are in `dist/`. Host the contents of that directory on a static web host. The app uses an origin-root base path; subfolder hosting such as GitHub Pages requires building with the appropriate `--base-href` and adjusting the font asset URL if necessary.

The Leaflet CommonJS warning is expected. It does not prevent the production build.

Browser checks to repeat after changes:

1. Switch between all three campuses and select a building.
2. Search for `Fenwick`, then try a search that has no results.
3. Save a place, reload, and confirm it appears in Saved places.
4. Open building details and directions, and check the origin and destination.
5. Use Tab, Enter, and Escape to navigate and close a dialog.
6. Turn on larger text, higher contrast, and reduced motion.
7. Use a narrow screen. Check the menu, map, list, and dialogs.

## Add the refresh to a new branch yourself

This version was based on original commit `2165a6a3a717cd888227022794acb4d80a3570f2`. The original GitHub repository has not been pushed to.

1. In your local copy of `campusable-week-5`, create a branch:

   ```bash
   git switch -c campusable-ui-refresh
   ```

2. Copy the refreshed `src/` and `tests/` folders and these files into your repository: `package.json`, `package-lock.json`, `angular.json`, `tsconfig.json`, `tsconfig.app.json`, and `README.md`. Keep your repository's `.git/` folder. Do not copy `node_modules/`, `dist/`, or `.openai/`.
3. Run `npm ci`, `npm test`, and `npm start`. Review the app locally.
4. When you are happy with it, commit and push your new branch:

   ```bash
   git add src tests package.json package-lock.json angular.json tsconfig.json tsconfig.app.json README.md
   git commit -m "Improve campus navigation and accessibility settings"
   git push -u origin campusable-ui-refresh
   ```

## Sources and credits

The four original Fairfax descriptions and their source links are preserved. Added campus information was checked against Mason's public sources on November 14, 2026:

- [Campus maps](https://www.gmu.edu/campus-maps)
- [Mason Square contact and location](https://masonsquare.gmu.edu/about/contact)
- [Mason building codes](https://registrar.gmu.edu/topics/building-codes/)
- [SciTech campus buildings](https://scitechcampus.gmu.edu/about/campus-buildings)
- [SciTech campus guide](https://scitechcampus.gmu.edu/about/frequently-asked-questions)

Map coordinates were checked against OpenStreetMap/Nominatim building records. Coordinates may change and do not identify an accessible entrance. No geocoding API is called while using the app.

Map: [Leaflet](https://leafletjs.com/) and [OpenStreetMap contributors](https://www.openstreetmap.org/copyright). Font: DM Sans, licensed under the SIL Open Font License; see `src/assets/DM-Sans-LICENSE.txt`. CampusAble is an independent student project, not an official George Mason University service.
