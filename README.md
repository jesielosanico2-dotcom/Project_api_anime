User flow (what the user sees)
Launch app → Login screen
Tap Login → Dashboard opens
Dashboard has 3 tabs:
Discover → shows featured manga (grid)
Search → search manga by title
Profile → app info + logout
Tap a manga → Manga Detail screen
On detail screen, tap a chapter → Chapter Reader (page images)
🌐 API Flow (what the app does behind the scenes)
Below is the sequence used by the app’s MangaDexService (in mangadex_service.dart):



Fetch featured manga → GET /manga?limit=20&includes[]=cover_art&availableTranslatedLanguage[]=en
Search manga → GET /manga?title=<query>&limit=20&includes[]=cover_art&availableTranslatedLanguage[]=en
Fetch manga details → GET /manga/<mangaId>?includes[]=cover_art
Fetch chapter list → GET /manga/<mangaId>/feed?limit=100&translatedLanguage[]=en&order[chapter]=desc
Fetch chapter pages → GET /at-home/server/<chapterId>
🧩 Mermaid Flow Diagram (API + UI)
🧱 Step-by-step: “How this API integration was built” (and how you can build it too)
1) Create Flutter project (if not already)
2) Add the HTTP package (for REST calls)
In pubspec.yaml:



Then run:



3) Define JSON models (parse API responses)
✅ In this repo:

manga_model.dart
chapter_model.dart
These map JSON responses into Dart objects like MangaModel and ChapterModel.

4) Build the API service layer
✅ In this repo: mangadex_service.dart
This class is responsible for:

Building the right MangaDex endpoint URL
Calling http.get(...)
Decoding JSON (jsonDecode)
Mapping to model objects
Throwing exceptions for non-200 responses
5) Connect UI screens to the service
✅ In this repo:




dashboard_screen.dart calls:
service.fetchFeaturedManga()
service.searchManga(query)
manga_detail_screen.dart calls:
service.fetchMangaDetails(mangaId)
service.fetchChapters(mangaId)
chapter_reader_screen.dart calls:
service.fetchChapterPages(chapterId
