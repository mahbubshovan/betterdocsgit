
Logo
BetterDocs E2E Test Automation (Lite)

Playwright test suite for BetterDocs plugin
About The Project

BetterDocs is a popular WordPress knowledge base plugin with thousands of users worldwide. It helps create and organize documentation, FAQs, knowledge bases, and glossaries with beautiful layouts, AI-powered chatbots, and instant answers.

This project provides end-to-end frontend automation testing for BetterDocs using Playwright. It covers Gutenberg blocks, Elementor widgets, shortcodes, instant answer, chatbot interactions, permalink routing, API endpoints, and more — all without requiring admin/backend access.

(back to top)
Built With

    Node
    Playwright

(back to top)
Getting Started
Prerequisites

    Node.js version 22 LTS
    npm

Installation

    Clone the repo

    git clone <repo-url>

    Install NPM packages

    npm install

    Create the .env file and provide necessary details

    cp .env.example .env

    Install Playwright browsers

    npx playwright install --with-deps

(back to top)
Usage

# Run all tests (headless)
npm test

# Run in headed mode (visible browser)
npx playwright test --headed

# Run by category
npm run test:blocks
npm run test:widgets
npm run test:shortcodes
npm run test:basic
npm run test:chatbot
npm run test:instant-answer

# View HTML report
npm run report

For more examples, refer to the Playwright Documentation

(back to top)
Project Structure

tests/
├── blocks/                      # Gutenberg block aria snapshot tests (39)
├── widgets/                     # Elementor widget aria snapshot tests (34)
├── shortcodes/                  # Shortcode aria snapshot tests (16)
├── basic/
│   ├── 404-checks/              # Page load / 404 verification across sites (10)
│   │   ├── cbotai-*             # Cbotai domain checks (docs, encyclopedia)
│   │   ├── msf-*                # MSF domain checks (docs, encyclopedia)
│   │   ├── docs-*               # Main site doc/category checks
│   │   ├── cross-domain         # Cross-domain parity (cbotai, msf, main)
│   │   └── intentional-404      # Verify 404 renders for invalid URLs
│   ├── permalink-routing/       # URL routing, SEO & structure tests (33)
│   │   ├── trailing-slash       # /docs vs /docs/ consistency
│   │   ├── pagination           # /docs/page/1/, out-of-range pages
│   │   ├── search-permalinks    # ?s=test&post_type=docs search URLs
│   │   ├── author-archive       # /docs/authors/ archive pages
│   │   ├── edge-cases           # Uppercase URLs, encoded chars, homepage
│   │   ├── sitemap-robots       # sitemap.xml & robots.txt availability
│   │   ├── canonical-urls       # Canonical URL consistency (strips UTM, multi-doc)
│   │   └── permalink-structure  # Trailing-slash redirect, double-slash, UTM, category & encyclopedia
│   ├── api-endpoints/           # Feed & REST API availability (3)
│   │   └── feed-api             # RSS feed, wp-json docs & doc_category
│   ├── card_based/              # Frontend regression tests (39)
│   │   ├── chatbot-style        # Launcher styling, color, hover, click
│   │   ├── deprecated-code      # .elementor-widget-container absence
│   │   ├── disable-js           # Verify removed scripts aren't loaded
│   │   └── nested-slug          # Nested category URL slug verification
│   ├── navigation/              # Interactive navigation tests (44)
│   │   ├── archive-pagination   # Pagination controls, page nav, doc links
│   │   ├── category-navigation  # Category box visibility, counts, nav
│   │   ├── doc-link-navigation  # Single doc clicks, category archives
│   │   ├── encyclopedia-nav     # A-Z filter, entry links, reset
│   │   ├── glossary-navigation  # Glossary tooltips, links, new tab nav
│   │   ├── faq-interaction      # FAQ expand/collapse behavior
│   │   ├── search               # Search bar, modal, live results
│   │   ├── search-filter        # Category dropdown, popular tags
│   │   └── sidebar-navigation   # Sidebar categories, icons, doc counts
│   ├── single-doc/              # Single doc & encyclopedia entry features (13)
│   │   ├── single-doc-features  # Breadcrumb, TOC, sidebar, prev/next, related
│   │   ├── article-reactions    # Thumbs up/down reactions, feedback form
│   │   └── encyclopedia-single  # Entry title, alphabet list, URL match
│   ├── seo/                     # SEO & meta tag tests (6)
│   │   └── meta-tags            # Title, canonical, H1, viewport meta
│   ├── accessibility/           # Accessibility tests (9)
│   │   ├── console-errors       # No JS console errors on 5 key pages
│   │   └── image-alt-text       # All images have alt attributes
│   ├── site-chrome/             # Header, footer, responsive (9)
│   │   ├── header-footer-nav    # Footer, logo home, main menu, skip link
│   │   └── mobile-viewport      # Mobile viewport rendering (375×812)
│   ├── docs                     # Docs page full aria snapshot (1)
│   └── encyclopedia             # Encyclopedia page full aria snapshot (1)
├── instant-answer/              # Instant answer widget - 4 tab tests (25)
│   ├── home-tab                 # Header, search input, docs list, tabs
│   ├── chatbot-tab              # Welcome message, email/guest login
│   ├── ask-tab                  # Query form: email, name, subject, upload
│   └── resources-tab            # Doc categories, Q&A section
├── chatbot/                     # AI chatbot interaction tests (10)
│   ├── chatbot-ui               # Panel title, response time, tabs
│   ├── guest-search             # Guest login, message, AI response + links
│   └── email-search             # Email login, search, response + links
└── helpers.js                   # Shared utilities (safeGoto, sendChatbotMessage, etc.)

Total: 301 tests across 137 files

(back to top)
What We Test
Snapshot Testing (89 tests)

Each Gutenberg block, Elementor widget, and shortcode page is visited once, and the full aria tree of main#content is captured using Playwright's toMatchAriaSnapshot(). This verifies the complete content structure — headings, links, categories, doc counts, icons, and text — without relying on visual pixel comparisons.
404 & Permalink Checks (46 tests)

Pages across three different sites (betteromation, betterdocs.msf, cbotai) are checked to ensure they load properly and don't return 404 errors. Additional permalink routing, SEO, and structure tests verify:

    Trailing slash consistency — /docs vs /docs/ both resolve correctly
    Pagination URLs — /docs/page/1/, category pagination, out-of-range pages return 404
    Search permalinks — ?s=test&post_type=docs search result pages load
    Author archives — /docs/authors/1/ loads without errors
    Edge cases — uppercase URLs, URL-encoded characters, homepage validation
    Cross-domain parity — same paths work across all three domains
    Intentional 404 validation — non-existent slugs and double slugs return HTTP 404
    API endpoints — RSS feed, REST API docs and category endpoints respond
    Sitemap & robots.txt — sitemap.xml returns XML, robots.txt returns 200
    Canonical URL consistency — single doc canonical strips UTM/query params, encyclopedia archive canonical matches URL, multi-doc canonical check (Cricket, Fencing, Apple)
    Trailing-slash redirect — /docs/cricket-the-gentlemens-game (no slash) redirects to /docs/cricket-the-gentlemens-game/
    Query-param resilience — UTM-tagged URLs don't break rendering
    Double-slash normalization — /docs//cricket-.../ normalizes correctly
    Category archive coverage — all real category archives (sports, fruits, team, qa) return 200
    Encyclopedia entry permalinks — 5 entries (aesthetic, altruism, ball, cat, dog) all resolve

Single Doc & SEO Tests (19 tests)

    Single doc features — Breadcrumb, home-link navigation, Table of Contents, sidebar, prev/next docs-nav, related articles
    Article reactions & feedback — Reaction buttons and feedback form presence
    Encyclopedia single entry — Entry title, alphabet navigation list, URL match
    Meta tags — Title tag, canonical URL, H1 presence on docs & encyclopedia, viewport meta tag

Accessibility & Site Chrome (18 tests)

    Console errors — No JS console errors on 5 key pages (homepage, docs, encyclopedia, single doc, single encyclopedia entry)
    Image alt text — All images have alt attributes on 4 key pages
    Header & footer — Footer visibility, logo→home navigation, main menu, skip-to-content link
    Mobile viewport (375×812) — Homepage renders without horizontal scroll, docs/encyclopedia/single doc render correctly on mobile

Interactive Frontend Tests (44 tests)

    Category Navigation: Verifies category boxes render correctly with names, doc counts, and icons. Clicks each category and confirms navigation to the correct archive page.
    FAQ Interaction: Tests expand/collapse behavior of FAQ sections, verifies multiple answers can be expanded simultaneously, and checks that answer content becomes visible on click.
    Search: Validates search bar visibility, placeholder text, modal opening on click, and live search results appearing when typing.
    Search Filter: Category dropdown selection, popular search tags visibility, submit button functionality on the block search page.
    Encyclopedia Navigation: A-Z alphabet filter links, clicking a letter filters entries, clicking All resets, entry links navigate to single encyclopedia pages.
    Archive Pagination: Pagination controls visibility, page 2 navigation, next arrow click, and doc list link click-through.
    Doc Link Navigation: Clicks docs from Sports and Fruits category archives, verifies single doc content loads, checks nested category archives.
    Sidebar Navigation: Sidebar category visibility with icons and doc counts across multiple sections.
    Glossary Navigation: Verifies glossary tooltip terms on a single doc page, checks tooltip data, confirms links point to encyclopedia entries, validates new-tab navigation, and ensures all glossary links are not 404.

Regression Tests (39 tests)

Adapted from the existing BDS_Automation admin test suite (CardBased folder):

    Chatbot Launcher Styling: Verifies launcher button background color, border-radius, hover color change, and panel opening behavior.
    Deprecated Code Check: Scans 12 pages to confirm .elementor-widget-container class is absent (regression for removed Elementor code).
    Removed Script Check: Scans 19 pages to verify extend-search-modal.js is no longer loaded.
    Nested URL Slugs: Validates that nested category URLs (/docs/team/qa/, /docs/sports/) resolve correctly.

Instant Answer Widget (25 tests)

The instant answer widget has 4 tabs: Home, Chatbot, Query, and Assets. Each tab is tested individually:

    Home: header text, search input, docs list, tab navigation
    Chatbot: welcome message, email login, guest login, field visibility
    Query: contact form fields (email, name, subject, message, file upload, send button)
    Assets: doc categories list, Q&A section with FAQ questions

AI Chatbot (10 tests)

    Guest Mode: Login as guest, type and send a message, verify AI responds and response links aren't 404.
    Email Mode: Login with msf@pro.automation, search "Fencing", verify AI responds and response links aren't 404.
    UI Elements: Panel title, response time info, description text, welcome message, send button, tab switching.

(back to top)
Error Handling & Resilience
Scenario 	How It's Handled
DB Connection Error 	safeGoto() detects "Error Establishing a Database Connection", waits 2 minutes, retries once
HTTP Error (502/500) 	safeGoto() catches page.goto() failures gracefully, test checks status code instead of page content
Chatbot "Thinking" 	sendChatbotMessage() polls for up to 1 minute waiting for AI response text to appear
Chatbot Technical Error 	If AI returns "experiencing" error, page is reloaded and the full chatbot flow is retried once
Code Snippet YAML 	Pages with raw code content (colons break YAML) use structural-only snapshots, skipping code text

(back to top)
Known Technical Considerations

    Aria snapshots are content-sensitive. If page content changes (new categories added, text modified, FAQ questions updated), the corresponding snapshot test will fail and needs to be regenerated. Run npx playwright test --update-snapshots to refresh.
    AI chatbot responses vary. The chatbot is powered by an external AI service. Response time, content, and availability can fluctuate. Tests verify that a response was received rather than checking exact response text.
    Code snippet pages contain raw source code with special characters (colons, backticks, curly braces) that are incompatible with YAML-based aria snapshots. These tests intentionally skip the code text and only verify structural elements.
    Server-side errors (502/500) on certain pages (e.g. /docs/team/) are server infrastructure issues, not test failures. Tests check HTTP status is not 404 rather than requiring 200.
    Headed vs headless mode may have minor timing differences. The test suite is optimized for both, but AI chatbot tests may take longer in headed mode due to rendering overhead.

(back to top)
Future Changes

This test suite is designed to evolve alongside BetterDocs:

    Snapshot regeneration will be needed when new features, layouts, or content changes are deployed. The generate-tests.js script in /scripts can bulk-regenerate snapshots for any category.
    New blocks/widgets/shortcodes can be added by appending URLs to the generator script and running it for the relevant category.
    Admin/backend tests could be introduced later with authentication setup (similar to the BDS_Automation project's auth.setup.js pattern).
    Additional chatbot scenarios (multi-turn conversations, different queries, edge cases) can be added using the sendChatbotMessage() helper.
    Cross-browser testing (Firefox, WebKit) can be enabled by adding projects to playwright.config.js.
    CI/CD integration via GitHub Actions is straightforward — a workflow template can be added to .github/workflows/.
    Instant answer tab names and content may change as the plugin is customized. Tests use flexible text matching where possible but may need updates after significant UI changes.

(back to top)
Roadmap

    All Gutenberg block snapshots
    All Elementor widget snapshots
    All shortcode snapshots
    Basic page snapshots (Docs, Encyclopedia)
    404 checks across multiple sites
    Cross-domain parity checks
    Permalink routing & edge case tests
    API endpoint availability checks
    Intentional 404 validation
    Category navigation tests
    FAQ interaction tests
    Search modal tests
    Instant answer (4 tabs)
    AI chatbot (guest + email login, response link validation)
    Frontend regression tests (deprecated code, removed scripts, URL slugs)
    Encyclopedia A-Z filter navigation
    Archive pagination navigation
    Doc link & category archive navigation
    Search category filter & popular tags
    Sidebar category navigation
    Glossary tooltip & encyclopedia link validation
    Single doc features (breadcrumb, TOC, reactions, related, feedback)
    Encyclopedia single-entry page tests
    SEO meta tag checks (title, canonical, H1, viewport)
    Accessibility: console errors & image alt text
    Header/footer navigation & skip-to-content link
    Mobile viewport responsive rendering
    Sitemap.xml & robots.txt availability
    Canonical URL consistency & query-param stripping
    Trailing-slash redirects, double-slash normalization
    CI/CD pipeline (GitHub Actions)
    Cross-browser support (Firefox, WebKit)
    Admin panel tests (with authentication)

(back to top)
Contact

Muammar Shahrear - @Muammar Shahrear - shahrearmuammar@gmail.com

(back to top)
About the Author

Muammar Shahrear is a software tester and researcher specializing in test automation, AI agents, WordPress plugin testing, and SaaS product quality assurance. He completed his B.Sc. and M.Sc. from the Institute of Information Technology (IIT), Jahangirnagar University (JU), Bangladesh, and also holds an M.Sc. from Technische Hochschule Mittelhessen (THM), Germany.

    LinkedIn
    Google Scholar

(back to top)
Acknowledgments

    Playwright
    BetterDocs
    othneildrew/Best-README-Template
