**Project Title**

Commerce Auctions Platform (Completed)

---

## Overview

This Django application implements a fully functional online auctions platform. Users can register, authenticate, create and browse listings, place bids, post comments, categorize items, and manage a personal watchlist. All core auction features are implemented and ready for deployment.

---

## Data Models (`auctions/models.py`)

* **User** (extends `AbstractUser`): default Django user fields plus any custom profile fields.
* **Category**: `name` field for auction grouping.
* **Listing**:

  * `title`, `description`, `starting_price`, optional `image_url`,
  * ForeignKey to `User` (creator), optional ForeignKey to `Category`,
  * `is_active` flag to close auctions.
* **Bid**:

  * `amount`, ForeignKey to `User` (bidder), ForeignKey to `Listing`, timestamp.
* **Comment**:

  * `content`, ForeignKey to `User` (author), ForeignKey to `Listing`, timestamp.

---

## URL Configuration (`auctions/urls.py`)

* **/** → `index` view: displays all active listings
* **/login/** → `login_view`: handles GET (render form) and POST (authenticate + redirect)
* **/logout/** → `logout_view`: logs out user
* **/register/** → `register`: handles user registration
* **/create/** → `create_listing`: form to create a new auction
* **/listing/[int\:id](int:id)/** → `view_listing`: shows details, current bid, comments, bid form, watchlist toggle
* **/categories/** → `view_categories`: list all categories
* **/categories/[str\:name](str:name)/** → `category_listings`: filter listings by category
* **/watchlist/** → `view_watchlist`: current user’s saved listings
* **/listing/[int\:id](int:id)/close/** → `close_listing`: closes auction, designates winner
* **/listing/[int\:id](int:id)/bid/** → `place_bid`: processes bid submissions
* **/listing/[int\:id](int:id)/comment/** → `add_comment`: posts new comment
* **/listing/[int\:id](int:id)/edit/** → `edit_listing`: allows creator to update listing details

---

## Views (`auctions/views.py`)

* **Authentication**:

  * `login_view`, `logout_view`, `register` handle user sessions and template context for `user.is_authenticated`.
* **Auction Management**:

  * `create_listing`: validates form input, creates Listing, redirects to homepage.
  * `view_listing`: retrieves listing, all bids (sorted), comments, highest bid, and watchlist status.
  * `place_bid`: enforces minimum bid > current highest, updates `Bid` model, refreshes page with success/error messages.
  * `add_comment`: saves a new `Comment` tied to listing.
  * `close_listing`: toggles `is_active=False`, determines and records winning bidder.
* **List and Filter**:

  * `index`: lists all active listings sorted by creation date.
  * `view_categories`: shows all categories with counts.
  * `category_listings`: filters `Listing` by category.
* **Watchlist**:

  * `view_watchlist`: displays listings the user has added.
  * Watchlist toggling handled within `view_listing`.

---

## Templates and Layout (`auctions/templates/auctions/layout.html`)

* Base layout includes navigation bar with dynamic links:

  * **Signed out**: Home, Login, Register
  * **Signed in**: Home, Create Listing, Watchlist, Categories, Logout (and “Signed in as {{ user.username }}”)
* Blocks for `title` and `body` content allow each view’s template to inject page-specific HTML.

---

## Usage and Running

1. **Install dependencies** (e.g., in virtual environment):

   ```bash
   pip install django
   ```
2. **Apply migrations** after model changes:

   ```bash
   python manage.py makemigrations auctions
   python manage.py migrate
   ```
3. **Run development server**:

   ```bash
   python manage.py runserver
   ```
4. **Interact via browser** on `http://127.0.0.1:8000/`: register, log in, create listings, bid, comment, and explore categories.


