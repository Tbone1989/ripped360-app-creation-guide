# Guide: Ripped360 App Layout, Design, and Home Screen in Glide

This guide continues from connecting your data to Glide and focuses on setting up the initial layout, design, and the Home screen for your Ripped360 app, based on your provided instructions.

**Prerequisites:**
*   You have successfully connected your "Ripped360 App Data" Google Sheet to Glide.
*   You are logged into your Glide account and have the Ripped360 app open in the Glide editor.

## PART 3: CONFIGURE APP LAYOUT & DESIGN

### Step 1: Configure Layout

1.  **Access Layout Editor:**
    *   In the Glide editor, the main navigation and screen configuration options are typically on the left side. Look for a section related to "Tabs", "Navigation", or "Layout".

2.  **Delete Default Tabs:**
    *   Glide often creates default tabs based on your data. If there are any pre-existing tabs you don't need, select them and look for an option to delete or remove them.

3.  **Add Your 5 Tabs:**
    *   Find the option to add new tabs (often a "+" icon or "Add Tab" button).
    *   Add the following five tabs one by one. For each tab, you will typically need to:
        *   **Name the tab:**
            *   Home
            *   Workouts
            *   Supplements
            *   Profile
            *   Settings
        *   **Link to a Data Source (initially, some might not be directly linked or will be configured later):** Glide will ask what data this tab should show. For now, you can assign them as follows, or leave some unlinked if the content will be custom built (like Home and Settings initially):
            *   **Home:** Often a custom screen. You might select "Custom Screen" or not link it to a specific sheet initially.
            *   **Workouts:** Link this to your "Workouts" table (data from the Workouts tab in your Google Sheet).
            *   **Supplements:** Link this to your "Supplements" table.
            *   **Profile:** This tab will usually be linked to the "Users" table and often has special "User Profile" configurations in Glide.
            *   **Settings:** Similar to Home, this might be a custom screen.

4.  **Set Appropriate Icons for Each Tab:**
    *   For each tab you've created, there should be an option to select an icon.
    *   Choose icons that best represent each tab's function:
        *   **Home:** Look for a house icon.
        *   **Workouts:** Look for a dumbbell, fitness, or exercise-related icon.
        *   **Supplements:** Look for a pill, capsule, or health-related icon.
        *   **Profile:** Look for a person, user, or avatar icon.
        *   **Settings:** Look for a gear or cogwheel icon.
    *   Save your tab configurations.

### Step 2: Set App Theme

1.  **Access Theme Settings:**
    *   In the Glide editor, look for a "Theme", "Appearance", or "Settings" section in the left sidebar that controls the overall look and feel of your app.

2.  **Choose "Dark" Theme:**
    *   Within the theme options, select the "Dark" theme if available. Glide usually offers Light, Dark, and sometimes other theme presets.

3.  **Set Primary Color to Red (#E63946):**
    *   Look for a color picker or input field for the "Primary Color" or "Accent Color".
    *   Enter the hex code `#E63946` for red. Some color pickers might allow you to select a color visually as well.

4.  **Upload Ripped360 Logo as App Icon:**
    *   In the same theme or app settings area, find the option to upload an "App Icon" or "Logo".
    *   Upload a simple version of your Ripped360 logo. This icon will be used for the app on users' home screens if they install it (for PWAs) and within Glide.

## PART 4: BUILD THE HOME SCREEN

### Step 1: Set Up Home Screen Components

1.  **Select the "Home" Tab:**
    *   In your app preview or tab editor, navigate to the "Home" tab you created.

2.  **Add Components:**
    *   Glide's interface allows you to add various components to a screen. Look for a "+" button or an "Add Component" panel, usually on the right side when a screen is selected.

    *   **Add "Rich Text" Component:**
        *   Select "Rich Text" from the list of available components.
        *   Configure its properties:
            *   **Title:** Set to "Welcome to Ripped360"
            *   **Description (or Body/Text):** Set to "Fitness that speaks your language"

    *   **Add "User Profile" Component:**
        *   Select a component designed for displaying user information. This might be called "User Profile", "Avatar Block", or similar.
        *   **Connect to Users Table:** Ensure this component is sourced from your "Users" table.
        *   **Display Fields:** Configure it to display the current user's "Name" and "PreferredLanguage" from the Users table. You'll typically select these columns from dropdowns in the component's settings.

    *   **Add "Dropdown" Component (for Language Selection):**
        *   Select the "Dropdown" or "Choice" component.
        *   Configure its properties:
            *   **Title (or Label):** Set to "Select Language"
            *   **Options:** Manually enter the language options: English, Spanish, Haitian Creole, Chinese, Japanese. (Glide usually allows you to add these as a list).
            *   **Save to (or Write to Column):** This is crucial. You need to save the user's selection. Your guide says: "Create a new User Specific Column named `UserLanguage`".
                *   In Glide's Data Editor (you might need to switch to it temporarily or find an option within the component settings), go to the "Users" table.
                *   Add a new column. Choose its type as "User-specific column" (this means each user has their own value for this column, and it's stored in Glide, not directly in your Google Sheet unless configured to write back, which is fine for this purpose).
                *   Name this new column `UserLanguage`.
                *   Go back to your Dropdown component on the Home screen and set it to save the selected language to this `UserLanguage` column in the Users table.

    *   **Add "Collection" Component (for Featured Workouts):**
        *   Select a "Collection" component (could be named "List", "Cards", "Gallery", etc., depending on the layout you choose).
        *   Configure its properties:
            *   **Data (Source):** Connect this to your "Workouts" table.
            *   **Filter:** Apply a filter to show only featured workouts. Your guide suggests `limit=3`. Look for filter options. You might need to add a specific column in your "Workouts" sheet like "IsFeatured" (True/False) to filter by, or Glide might have an option to limit the number of items displayed directly (e.g., "Show first 3 items"). If there isn't a direct limit option, you might need to sort your workouts and then Glide will show the first few. For a true "featured" system, adding a dedicated "IsFeatured" column in your Google Sheet is more robust.
            *   **Layout:** Choose "Cards" or a similar image-centric layout.
            *   **Image:** Set the image for each card to use the `ImageURL` column from your Workouts table.
            *   **Title:** Initially, set the title to show the `WorkoutName_EN` column. (You will make this dynamic in the next step).

### Step 2: Create Computed Column for Multilingual Content (in Glide Data Editor)

This step involves modifying your data tables *within Glide* to add columns that dynamically change based on the selected language.

1.  **Go to "Data Editor":**
    *   Find the "Data" or "Data Editor" tab/button in the Glide interface (usually at the top or prominent in the left navigation).

2.  **Select "Workouts" Table:**
    *   In the Data Editor, choose your "Workouts" table.

3.  **Add Computed Column: "WorkoutName_Display"**
    *   Click "Add Column" or a similar option to create a new column in the Workouts table (this column exists in Glide, not directly in your Google Sheet).
    *   Choose the column type as "Computed Column" or "If-Then-Else" column, or a similar logic-based column type. Glide has various ways to achieve this.
        *   **Name it:** `WorkoutName_Display`
        *   **Configure the Logic/Formula:** You need to implement the following logic:
            `IF UserLanguage (from User Specific Columns in Users table) IS 


            `UserLanguage` (this is the User Specific Column you created in the Users table) IS "Spanish" THEN `WorkoutName_ES` (from the current Workouts table row)
            ELSE IF `UserLanguage` IS "Haitian Creole" THEN `WorkoutName_HC`
            ELSE IF `UserLanguage` IS "Chinese" THEN `WorkoutName_ZH`
            ELSE IF `UserLanguage` IS "Japanese" THEN `WorkoutName_JA`
            ELSE `WorkoutName_EN` (this is the default value if no other condition is met)
            Glide's interface for If-Then-Else columns will guide you in setting up these conditions and selecting the corresponding columns from your Workouts table and the UserLanguage from the user's profile.

4.  **Create Computed Column: "Description_Display"**
    *   Similarly, add another Computed Column to the "Workouts" table.
    *   Name it: `Description_Display`
    *   Configure the logic/formula like `WorkoutName_Display`, but using the description columns (`Description_EN`, `Description_ES`, etc.):
        `IF UserLanguage IS "Spanish" THEN Description_ES`
        `ELSE IF UserLanguage IS "Haitian Creole" THEN Description_HC`
        `ELSE IF UserLanguage IS "Chinese" THEN Description_ZH`
        `ELSE IF UserLanguage IS "Japanese" THEN Description_JA`
        `ELSE Description_EN`

5.  **Update Home Screen Collection:**
    *   Go back to the Layout editor and select the "Home" tab.
    *   Select the "Collection" component displaying featured workouts.
    *   In its properties, change the field bound to the **Title** to use your new `WorkoutName_Display` computed column.
    *   If your cards also show a description, update that to use the `Description_Display` column.

## PART 5: BUILD THE WORKOUTS SCREEN

### Step 1: Set Up Workouts List Screen

1.  **Select the "Workouts" Tab:**
    *   In the Layout editor, navigate to the "Workouts" tab you created.
    *   Ensure this tab is linked to your "Workouts" table as its source.

2.  **Add "Collection" Component (or ensure one is present):**
    *   Glide likely added a default collection/list component when you linked the tab to the Workouts table. If not, add one.
    *   Configure its properties:
        *   **Data:** Should already be set to the "Workouts" table.
        *   **Layout:** Choose "List with images" or a similar layout that suits your content (e.g., Cards, Compact List).
        *   **Fields to Show (Customize the items in the list):**
            *   **Title:** Set to `WorkoutName_Display` (your computed column).
            *   **Description/Subtitle:** Set to `Description_Display` (your computed column).
            *   Add other relevant fields like `Duration` and `DifficultyLevel`.
            *   **Image:** Set to use the `ImageURL` column from your Workouts table.
        *   **Action on Click:** By default, clicking an item in a collection usually navigates to a "Detail Screen" for that item. Ensure this is the case.

### Step 2: Create Workout Details Screen

When a user taps on a workout in the list, they should see a more detailed view. Glide usually auto-creates a basic detail screen. You need to customize it.

1.  **Access Detail Screen Configuration:**
    *   Click on an item in your Workouts list preview to navigate to its detail screen. You should now be able to edit the layout of this detail screen.

2.  **Add Components to the Detail Screen:**
    *   **Image Component:**
        *   Add an "Image" component at the top.
        *   Set its data source to the `ImageURL` column of the current workout item.
    *   **Title Component (or Rich Text/Text Component):**
        *   Add a "Title" or "Text" component.
        *   Set its data source to `WorkoutName_Display`.
    *   **Description Component (or Rich Text/Text Component):**
        *   Add a "Text" or "Rich Text" component.
        *   Set its data source to `Description_Display`.
    *   **Statistics (e.g., using "Text" components, "Key-Value" pairs, or a "Fields" component):**
        *   Display `Duration`, `CaloriesBurn`, and `DifficultyLevel`. You might add these as separate text components or use a component that allows multiple field-value pairs.

    *   **Add "Button" Component to "Start Workout":**
        *   Add a "Button" component.
        *   **Label/Text:** Set to "Start Workout" (This text itself isn't multilingual in this step, but the app can be enhanced later).
        *   **Configure Action:** This is a key step.
            *   Set the button's action to "Add Row" or "Create New Row".
            *   **Target Sheet/Table:** Select your "UserWorkouts" table.
            *   **Set Auto-Values (Map Columns):** You need to automatically populate columns in the new `UserWorkouts` row:
                *   `UserID`: Set this to the `UserID` of the currently signed-in user (Glide usually has a special value for "User's UserID" or similar from the user profile).
                *   `WorkoutID`: Set this to the `WorkoutID` of the current workout item being viewed (from the detail screen's context).
                *   `DateCompleted`: Set this to the current date/time (Glide often has a special value for "Current Date/Time" or Today()).
                *   Other fields like `Duration`, `IntensityRating`, `Notes`, `Progress` in `UserWorkouts` would typically be filled in by the user *after* starting or completing the workout, perhaps on a subsequent screen or by editing the logged workout. For now, logging the start is the primary goal.

## PART 6: BUILD THE SUPPLEMENTS SCREEN

### Step 1: Set Up Supplements List

1.  **Select the "Supplements" Tab:**
    *   Navigate to the "Supplements" tab in the Layout editor.
    *   Ensure it's linked to your "Supplements" table.

2.  **Create Computed Columns for Multilingual Supplement Content (in Data Editor):**
    *   Go to the "Data Editor" and select the "Supplements" table.
    *   Create the following computed columns, similar to how you did for Workouts:
        *   `SupplementName_Display`: Based on `UserLanguage` and `SupplementName_EN`, `SupplementName_ES`, etc.
        *   `Purpose_Display`: Based on `UserLanguage` and `Purpose_EN`, `Purpose_ES`, etc.
        *   `Warnings_Display`: Based on `UserLanguage` and `Warnings_EN`, `Warnings_ES`, etc.

3.  **Add "Collection" Component:**
    *   Add or configure the collection component on the Supplements screen.
    *   **Data:** "Supplements" table.
    *   **Layout:** "List with icons" (or another suitable layout).
    *   **Fields to Show:**
        *   **Title:** `SupplementName_Display`.
        *   You might add other brief info here, like `Category`.

4.  **Add Color-Coding for Safety (Conditional Formatting):**
    *   Select the Collection component.
    *   Look for "Conditional Formatting" or "Styling Rules" options within the component's settings or for its items.
    *   Add conditions based on the `SafetyRating` column from your Supplements table:
        *   **Condition 1:** IF `SafetyRating` IS "Red" THEN Set item **Background Color** to Red (e.g., `#FF0000` or a Glide color preset).
        *   **Condition 2:** IF `SafetyRating` IS "Yellow" THEN Set item **Background Color** to Yellow (e.g., `#FFFF00`).
        *   **Condition 3:** IF `SafetyRating` IS "Green" THEN Set item **Background Color** to Green (e.g., `#00FF00`).
        *   (Adjust colors for better UI/UX, especially with a dark theme. The hex codes are examples.)

### Step 2: Create Supplement Detail Screen

1.  **Access Detail Screen Configuration:**
    *   Click on an item in your Supplements list preview to navigate to its detail screen for editing.

2.  **Add Components to the Detail Screen:**
    *   **Image Component:** Source: `ImageURL` from Supplements table.
    *   **Title Component:** Source: `SupplementName_Display`.
    *   **Description/Purpose Component (Text/Rich Text):** Source: `Purpose_Display`.
    *   **Warning Section (Text/Rich Text):**
        *   Add a Text or Rich Text component.
        *   Source: `Warnings_Display`.
        *   You might want to add a static label like "Warnings:" above this component if the `Warnings_Display` itself doesn't include it.
    *   **Contraindications Section (Text/Rich Text):**
        *   Add a Text or Rich Text component.
        *   Source: `ContraIndications` column (this column wasn't specified as multilingual in the headers, so using it directly. If it needs to be multilingual, you'd create a `Contraindications_Display` computed column).

## PART 7: BUILD PROFILE & SETTINGS SCREENS

These screens are often simpler or use more of Glide's built-in user-related components.

### Step 1: Profile Screen

1.  **Select the "Profile" Tab:**
    *   Navigate to the "Profile" tab.
    *   Ensure it's linked to the "Users" table.

2.  **Add Components:**
    *   **User Profile Component:**
        *   Glide often has a dedicated "User Profile" component or allows you to easily display fields from the current user's row in the Users table (e.g., Name, Email, JoinDate, FitnessGoals, CurrentPlan).
        *   Add this and configure it to show the desired user information.
    *   **Statistics about Completed Workouts:**
        *   This requires more advanced data handling. You'd typically need to:
            *   Create a "Relation" in the Data Editor from the "Users" table to the "UserWorkouts" table (linking `UserID` in both).
            *   Then, in the Users table, create "Rollup" columns that calculate statistics over this relation (e.g., Count of UserWorkouts, Sum of Duration from UserWorkouts).
            *   Display these Rollup columns on the Profile screen using Text components.
    *   **Collection of Past Workouts:**
        *   Add a "Collection" component.
        *   Source: "UserWorkouts" table.
        *   **Filter:** Filter this collection to show only workouts where `UserID` matches the `UserID` of the currently signed-in user.
        *   Display relevant fields like `WorkoutID` (you might want to show `WorkoutName_Display` by creating a relation from UserWorkouts to Workouts and then a Lookup column), `DateCompleted`, `Duration`, `IntensityRating`.

### Step 2: Settings Screen

1.  **Select the "Settings" Tab:**
    *   Navigate to the "Settings" tab. This tab might not be directly linked to a specific table, or it could be linked to the Users table if many settings are user-specific.

2.  **Add Components:**
    *   **Language Preference Dropdown:**
        *   You already added a language dropdown on the Home screen. You can add a similar one here.
        *   Add a "Dropdown" component.
        *   Title: "Select Language" (or similar).
        *   Options: English, Spanish, Haitian Creole, Chinese, Japanese.
        *   Save to: The same `UserLanguage` User Specific Column in the Users table.
    *   **App Theme Settings:**
        *   Glide might have built-in components or actions to allow users to switch between Light/Dark themes if you've enabled them in the app's settings. If not, this is a more advanced feature to implement manually.
    *   **Notification Preferences:**
        *   This typically requires columns in your Users table (e.g., `EmailNotificationsEnabled` as a boolean/checkbox) and then components (like a "Switch" or "Checkbox") on this screen linked to those columns.
    *   **About Section:**
        *   Add a "Rich Text" or "Text" component.
        *   Enter information about your app (e.g., version, developer, contact).

## PART 8: ADD APP LOGIC & BASIC AUTOMATIONS

This involves using Glide's "Actions" editor.

### Step 1: Set Up Language Change Logic

This is mostly handled by the Dropdown component saving directly to the `UserLanguage` user-specific column. The computed columns then react automatically.

1.  **Verify Dropdown Behavior:**
    *   Ensure the language dropdowns on the Home and Settings screens correctly write their selection to the `UserLanguage` column in the Users table for the current user.
2.  **No Separate "Action" Needed (Usually):**
    *   Glide's components that write to a column (like the Dropdown saving to `UserLanguage`) handle this directly. The computed columns that *read* `UserLanguage` will update automatically when `UserLanguage` changes.
    *   The guide mentions: "Create new action: 'Change Language', Set trigger: When user changes language dropdown, Set action: Update UserLanguage column for current user." This is implicitly what the Dropdown component does when configured to save to the `UserLanguage` column. If your Glide version requires an explicit action for this, you would:
        *   Go to the "Actions" editor.
        *   Create a new action.
        *   The trigger would be the dropdown component changing.
        *   The action step would be "Set Column Value" or "Update Row", targeting the current user's row in the Users table, setting the `UserLanguage` column to the value from the dropdown.
        *   However, modern Glide usually handles this more directly via component data binding.

### Step 2: Create Sign-In Flow

1.  **Go to App Settings -> Sign-in:**
    *   In the Glide editor, find the main app "Settings" (often a gear icon at the top level, not the Settings tab you are building *within* the app).
    *   Navigate to the "Sign-in" or "Privacy" or "User Authentication" section.

2.  **Enable Email Sign-in:**
    *   Choose an option like "Public with email" or "Email whitelist" or simply enable email sign-in.
    *   Glide will handle the sign-in screen, verification codes, etc.

3.  **Configure to Save User Info to Users Table:**
    *   Ensure that Glide is configured to use your "Users" table as the destination for user profiles.
    *   When a new user signs up, Glide should add a new row to this table.
    *   Map the default user properties (like email) to the correct columns in your Users table (e.g., Glide's user email to your `Email` column).

### Step 3: Add Safety Warning Logic (for Supplements)

This action is triggered when a user views supplement details if the safety rating is "Red".

1.  **Go to the Supplements Detail Screen:**
    *   Navigate to the detail screen configuration for your Supplements collection.

2.  **Configure an "On View" Action or Conditional Visibility/Component:**
    *   **Method 1: Conditional Action on Screen Load (if Glide supports this directly for detail screens):**
        *   Look for options to add an action that runs when the screen is opened/viewed.
        *   Add a condition to this action: IF `SafetyRating` (of the current supplement) IS "Red".
        *   Then, add an action step: "Show Notification", "Show Alert", or "Show Custom Screen/Popup".
        *   The message should be a warning (e.g., "High Safety Risk! Please consult a professional.").

    *   **Method 2: Conditional Navigation or Overlay (More Common):**
        *   When a user clicks a supplement in the list, instead of directly going to the full detail screen, the action could first go to a *conditional* action.
        *   In the "Actions" editor, create a new compound action for when a supplement item is clicked:
            *   **Name:** e.g., "ViewSupplementWithWarning"
            *   **Step 1 (Conditional):** IF `SafetyRating` of the clicked supplement IS "Red":
                *   THEN: "Show Notification" (with warning message) OR "Navigate to a specific warning screen/popup". After the warning, you might then navigate to the actual detail screen or require an acknowledgment.
            *   **Step 2 (Or part of Else):** Navigate to the standard Supplement Detail Screen.
            *   This can be structured as: IF Red -> Show Warning THEN Go to Details. ELSE -> Go to Details.

    *   **Method 3: Conditional Visibility of a Warning Component on the Detail Screen:**
        *   On the Supplement Detail Screen itself, add a prominent "Rich Text" or "Warning Box" component at the top.
        *   Set its content to the warning message.
        *   Set its **Visibility Condition**: Only show this component IF `SafetyRating` IS "Red".
        *   This is often the simplest to implement directly on the screen.

Your guide says: "Set trigger: When viewing supplement details. If SafetyRating = "Red", show warning message before displaying details." Method 2 (conditional action before navigation) or a variation where the detail screen itself has an entry action is closest. Method 3 (conditional visibility on the detail screen) is also a good way to achieve the warning.

This covers the core of building the features. The next parts are about publishing and the website.




## PART 9: PUBLISH & SHARE YOUR APP (IN GLIDE)

Once you've built and configured the screens and logic, it's time to test thoroughly and publish your Glide app.

### Step 1: Preview Your App

1.  **Use Glide's Preview Mode:**
    *   Glide has a robust preview mode that allows you to interact with your app as a user would, on different device sizes (phone, tablet).
    *   Click the "Preview" button or toggle (often at the top of the Glide editor).
2.  **Test All Features Thoroughly:**
    *   **Language Switching:** Change the language using your dropdown. Verify that all content you configured with `_Display` computed columns (Workout Names, Descriptions, Supplement Names, Purposes, Warnings) changes accordingly.
    *   **Workout Tracking:** Go to the Workouts screen, select a workout, and click "Start Workout". Check if a new row is added to your "UserWorkouts" table in the Glide Data Editor (and subsequently in your Google Sheet, depending on Glide's sync settings).
    *   **Supplement Info & Safety:** Browse supplements. Check if the color-coding for safety ratings works. For supplements with a "Red" safety rating, verify that your warning logic (e.g., conditional warning message or overlay) appears as expected before or on the detail screen.
    *   **Navigation:** Test all tabs and buttons. Ensure navigation flows logically.
    *   **User Profile and Settings:** Check if user data is displayed correctly on the profile. Test any settings you've implemented.
    *   **Sign-in/Sign-up:** Log out and try signing up as a new user and signing back in.

### Step 2: Publish Your App

1.  **Click the "Publish" Button:**
    *   In the Glide editor, there's usually a prominent "Publish" or "Share" button (often at the top right).

2.  **Configure Visibility:**
    *   Glide will ask you to configure your app's link and privacy settings.
    *   **Link:** You can often customize the app's URL (e.g., `your-app-name.glideapp.io`).
    *   **Visibility/Privacy:** Your guide specifies "Public". Choose the appropriate setting that makes the app accessible to anyone with the link. Glide might have options like "Public", "Public with sign-in", "Email whitelist", etc. "Public with sign-in" is common if you want users to have accounts.

3.  **Get Your App URL and QR Code:**
    *   Once published, Glide will provide you with the direct URL for your app.
    *   It will also typically generate a QR code that users can scan to access the app on their mobile devices easily.
    *   Copy these and save them.

## FINAL STEPS (POST-GLIDE APP PUBLISHING)

1.  **Share your app:**
    *   Send the app URL and/or QR code to potential users, testers, stakeholders, or include it in your grant applications.

2.  **Monitor usage:**
    *   Glide has built-in analytics (usually available on paid plans, but basic stats might be on the free plan).
    *   Check these analytics to see how users are engaging with your app, which features are popular, etc.

3.  **Iterate:**
    *   Based on user feedback and your own observations, continue to improve your app.
    *   Add more content (workouts, supplements), enhance features, and fix any bugs.
    *   Glide allows you to update your app, and changes are usually live quickly after you make them in the editor (though you might need to re-publish or changes sync automatically).

4.  **Apply for grants:**
    *   Use your functional prototype as a key part of your grant applications. A working app is much more compelling than just an idea.

## WEBSITE CREATION (USING GOOGLE SITES)

Your second guide mentions creating a simple website for Ripped360. Here's how you can do that using Google Sites:

### Step 1: Go to Google Sites

*   Navigate to [Google Sites](https://sites.google.com).
*   Sign in with your Google account.

### Step 2: Create a New Site

*   Click on "Blank" or choose a template to start a new site.

### Step 3: Build Basic Site Structure & Content

1.  **Site Name & Logo:**
    *   Enter "Ripped360" as your site name (usually at the top left).
    *   Add your Ripped360 logo. Google Sites allows you to upload a logo in the header settings.

2.  **Homepage Content:**
    *   Use the right-hand insert panel to add text boxes, images, and other elements.
    *   **App Description:** Write a compelling description of the Ripped360 app, its purpose, and who it's for.
    *   **Features:** Highlight the key features (multilingual support, workout tracking, supplement safety info, etc.). Use bullet points or distinct sections.
    *   **Founder Story (Optional but good for grants):** Briefly share the motivation behind creating Ripped360.
    *   **Screenshots of your Glide app:** Take some attractive screenshots of your Ripped360 Glide app in action and insert them onto your Google Site page to showcase what the app looks like.

3.  **Add App Link and QR Code:**
    *   **Button:** Insert a "Button" element from the insert panel.
        *   **Name:** e.g., "Access the Ripped360 App" or "Launch App".
        *   **Link:** Paste the URL of your published Glide app.
    *   **QR Code:** Insert an "Image" element and upload the QR code for your Glide app.
    *   Make these calls to action prominent.

4.  **Add Other Pages (Optional):**
    *   You can add more pages like "About Us", "Contact", "FAQ" using the "Pages" tab in the right-hand panel.

### Step 4: Publish Your Google Site

1.  **Click "Publish"** (top right corner).
2.  **Choose your web address:** Google Sites will provide a URL like `sites.google.com/view/your-site-name`. You can customize `your-site-name`.
3.  **Manage site visibility:** Ensure it's set to public if you want everyone to see it.
4.  Click "Publish" again.

## ADDITIONAL STEPS FOR GROWTH

These are ongoing activities to help your app succeed:

1.  **Set up a Simple Analytics System:**
    *   **Google Analytics for your Google Site:** Google Sites has a built-in option to add a Google Analytics tracking ID (in Site Settings). This will track visitors to your website.
    *   **Glide's Built-in Analytics:** As mentioned, utilize Glide's analytics for app-specific usage data.

2.  **Create Social Media Presence:**
    *   Set up accounts on relevant platforms (e.g., Instagram, Facebook, TikTok, X/Twitter) for Ripped360.
    *   Share information about the app, screenshots, user testimonials (if you get them), fitness tips related to your app's content, etc.
    *   Link to your Google Site and directly to the Glide app from your social media profiles.

3.  **Prepare for Grant Applications (Ongoing):**
    *   **Document Your Technical Implementation:** Keep notes on how you built the app (even if it's no-code with Glide, explain your data structure, logic, etc.). This guide itself is a good start!
    *   **Gather User Feedback:** Actively solicit feedback from early users. This is invaluable for iteration and for showing traction in grant applications.
    *   **Track Usage Metrics:** Use Glide analytics and website analytics to demonstrate user engagement and growth over time.

This completes the guidance for publishing your app, creating a promotional website, and initial steps for growth. Remember to update your `todo.md` as you complete these manual steps.

