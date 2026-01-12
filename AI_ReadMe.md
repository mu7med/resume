# AI ReadMe - Architecture & Codebase Context

## Project Metadata
- **Project Name**: Resume / Personal Portfolio Webpage
- **Type**: Static Web Application
- **Generator**: [Mu7med](https://mu7med.com)
- **Tech Stack**:
    - **Markup**: HTML5
    - **Styling**: CSS3 (Nicepage framework + Custom overrides)
    - **Scripting**: JavaScript (jQuery 1.9.1 based)
- **Key Libraries**:
    - `nicepage.js`: Core framework for layout, animations, and interactive components.
    - `jquery.js`: DOM manipulation dependency.
    - `intlTelInput`: International telephone input handling.

## File System Topology
```
./resume
├── index.html                  # Entry point (Main Landing Page)
├── Home.html                   # Duplicate/Main Content Source
├── nicepage.js                 # Core Framework Logic (Minified/Bundled)
├── nicepage.css                # Core Framework Styles
├── Home.css                    # Page-specific styles for Home
├── jquery.js                   # jQuery Library
├── README.md                   # User documentation
├── blog/                       # Blog Sub-system
│   ├── blog.html               # Blog Index / Listing Page
│   ├── post.html               # Individual Post Templates
│   ├── post-1.html ... 5.html  # Generated Blog Post Pages
│   └── ...
├── images/                     # Static Image Assets (Profile pics, backgrounds)
├── files/                      # Downloadable Assets (Resume.pdf)
├── intlTelInput/               # Telephone Input Library Assets
│   ├── intlTelInput.min.js
│   └── ...
└── .git, .github               # Version Control Configuration
```

## Detailed Codebase Analysis

### Root Directory
#### [index.html](file://./index.html)
- **Role**: The main entry point for the website.
- **Key Components**:
    - Structurally identical to `Home.html`.
    - Loads `nicepage.css`, `Home.css`, `jquery.js`, and `nicepage.js`.
    - Contains JSON-LD structured data for `Organization`.
    - **Sections**:
        1. **Hero**: Profile introduction (Name, Role, Contact Info).
        2. **Experience**: Timeline/List of work history (Sudan info network, Injazat, Oracle, Halian).
        3. **About Me**: Narrative biography and skill statistics (charts).
        4. **Footer/Video**: Embedded YouTube video and "Unifying software lifecycle" section.
- **Data Flow**: Static HTML content. Contact links use `mailto:` and `tel:`.
- **Dependencies**: `nicepage.css`, `Home.css`, `jquery.js`, `nicepage.js`.

#### [Home.html](file://./Home.html)
- **Role**: Serves as the primary content definition, likely the source used by the generator to build `index.html`.
- **Content**: Duplicate of `index.html`.

#### [nicepage.js](file://./nicepage.js)
- **Role**: The engine driving the UI interactions.
- **Key Functions**:
    - **Responsive Design**: Handles viewport scaling and `u-responsive-*` classes.
    - **Form Handling**: Manages form submission, validation, and integration with Nicepage backend services.
    - **Animations**: Orchestrates scroll animations (`customAnimationIn`) and parallax effects.
    - **Lightbox/Gallery**: Manages image zoom and gallery carousel behaviors (using PhotoSwipe).
    - **ScrollSpy**: Updates active navigation links based on scroll position.

### Blog Sub-system (`./blog/`)
#### [blog/blog.html](file://./resume/blog/blog.html)
- **Role**: The index page for the blog section.
- **Architecture**: A generated list of blog posts.
- **Key Components**:
    - Repeater Grid (`u-repeater`): Displays thumbnails and summaries of posts.
    - Links to individual posts (e.g., `../blog/post-1.html`).
- **Dependencies**: Relies on parent directory assets (`../nicepage.css`, `../nicepage.js`).

#### [blog/post-*.html](file://./resume/blog/)
- **Role**: Individual static blog post pages.
- **Pattern**: Generated from a template (`post.html` likely acting as the base schema), containing specific content for each article.

### Assets & Config
- **`Home.css`**: Contains specific style rules for the unique IDs and classes used in the Home page (e.g., `.u-section-1`, animation parameters).
- **`nicepage.css`**: The global CSS framework defining the grid system, typography, and utility classes (`u-clearfix`, `u-sheet`, etc.).
- **`files/Resume.pdf`**: The binary artifact linked for download in the Hero section.

## Architecture & Data Flow
1.  **Initialization**: Browser loads `index.html`. `jquery.js` initializes, followed by `nicepage.js`.
2.  **Rendering**: `nicepage.js` scans the DOM for `u-*` classes to attach event listeners (scroll, resize, click).
3.  **Interaction**:
    - **Navigation**: Standard anchor links.
    - **Forms**: `nicepage.js` intercepts `form` submissions, likely sending data to Nicepage's hosted form service or a configured email endpoint (if valid).
    - **Animations**: Scroll events trigger CSS variable updates to drive animations defined in `nicepage.css` and `Home.css`.
4.  **State Management**: Purely DOM-based. No client-side router or global state store (Redux/Context) is present. State is ephemeral per page load.

## Critical Logic & Edge Cases
- **Responsive Handling**: The site uses a JS-driven responsiveness system (`u-xl-mode`, `u-palette-*`) combined with CSS media queries. Modifying the HTML structure without updating the `data-*` attributes expected by `nicepage.js` may break layouts.
- **Form Dependencies**: The form submission logic (`11795` module in `nicepage.js`) explicitly checks for GDPR cookie consent and reCAPTCHA. Form submissions might fail if these external scripts are blocked or not configured.
- **Path Resolution**: Blog pages rely on relative paths (`../`). Moving files requires careful link updates.
