# Ripped360 App Creation Checklist

## PART 1: SET UP YOUR DATA SOURCE

- [ ] **Step 1: Create the Google Sheet (Manual User Task)**
  - [ ] Go to Google Sheets and create a new spreadsheet.
  - [ ] Rename the spreadsheet to "Ripped360 App Data".
  - [ ] Rename "Sheet1" to "Users".
  - [ ] Create additional tabs: Workouts, Exercises, Supplements, UserWorkouts, UserSupplements.

- [x] **Step 2: Add Column Headers (Automated - CSVs Provided)**
  - [x] Users Tab Headers: `UserID | Name | Email | PreferredLanguage | JoinDate | FitnessGoals | CurrentPlan`
  - [x] Workouts Tab Headers: `WorkoutID | WorkoutName_EN | WorkoutName_ES | WorkoutName_HC | WorkoutName_ZH | WorkoutName_JA | Description_EN | Description_ES | Description_HC | Description_ZH | Description_JA | DifficultyLevel | Category | Duration | CaloriesBurn | ImageURL`
  - [x] Exercises Tab Headers: `ExerciseID | ExerciseName_EN | ExerciseName_ES | ExerciseName_HC | ExerciseName_ZH | ExerciseName_JA | Description_EN | Description_ES | Description_HC | Description_ZH | Description_JA | MuscleGroup | Equipment | DifficultyLevel | ImageURL`
  - [x] Supplements Tab Headers: `SupplementID | SupplementName_EN | SupplementName_ES | SupplementName_HC | SupplementName_ZH | SupplementName_JA | Category | Purpose_EN | Purpose_ES | Purpose_HC | Purpose_ZH | Purpose_JA | SafetyRating | Warnings_EN | Warnings_ES | Warnings_HC | Warnings_ZH | Warnings_JA | ContraIndications | ImageURL`
  - [x] UserWorkouts Tab Headers: `LogID | UserID | WorkoutID | DateCompleted | Duration | IntensityRating | Notes | Progress`
  - [x] UserSupplements Tab Headers: `LogID | UserID | SupplementID | DosageAmount | Frequency | StartDate | ReminderTime | Notes`

- [ ] **Step 3: Copy the Data (Manual User Task)**
  - [ ] User to copy and paste provided data into each respective tab in Google Sheets, aligning columns with headers.

- [ ] **Step 4: Auto-ID Formula (Manual User Task - Optional)**
  - [ ] User to add formula `=IF(LEN(B2),"U"&TEXT(ROW()-1,"000"),"")` to cell A2 of Users tab in Google Sheets and drag down.

- [x] **Step 1 (from second guide): Download the Data as CSV Files (Partially Automated - CSVs created, user to import to Google Sheets then export if preferred, or use directly)**
  - [ ] Create a New Google Sheet (Manual User Task - covered above)
  - [ ] Create Multiple Tabs (Manual User Task - covered above)
  - [ ] Copy and Paste the Data (Manual User Task - covered above)
  - [x] Export as CSV Files (Automated - CSVs will be provided for each sheet structure. User will populate them with data.)

## PART 2: CREATE YOUR APP WITH GLIDE (Manual User Task)

- [ ] **Step 1: Sign Up for Glide**
  - [ ] Go to Glide and sign up for a free account.
  - [ ] Verify email and log in.

- [ ] **Step 2: Create a New App**
  - [ ] Click "New App".
  - [ ] Select "Start with data" (or "Start from spreadsheet").
  - [ ] Choose "Google Sheets".
  - [ ] Select "Ripped360 App Data" spreadsheet.
  - [ ] Click "Create App".

- [ ] **Step 3: Rename Your App**
  - [ ] Change default app name to "Ripped360".
  - [ ] Click "Save".

## PART 3: CONFIGURE APP LAYOUT & DESIGN (Manual User Task)

- [ ] **Step 1: Configure Layout**
  - [ ] Delete default tabs.
  - [ ] Add 5 tabs: Home, Workouts, Supplements, Profile, Settings.
  - [ ] Set icons: Home (House), Workouts (Dumbbell/exercise), Supplements (Pill/capsule), Profile (Person), Settings (Gear).

- [ ] **Step 2: Set App Theme**
  - [ ] Choose "Dark" theme.
  - [ ] Set primary color to red (#E63946).
  - [ ] Upload Ripped360 logo as app icon.

## PART 4: BUILD THE HOME SCREEN (Manual User Task)

- [ ] **Step 1: Set Up Home Screen**
  - [ ] Add "Rich Text" component: Title "Welcome to Ripped360", Description "Fitness that speaks your language".
  - [ ] Add "User Profile" component: Connect to Users table, Display Name and PreferredLanguage.
  - [ ] Add "Dropdown" component: Title "Select Language", Options (English, Spanish, Haitian Creole, Chinese, Japanese), Save to new User Specific Column "UserLanguage".
  - [ ] Add "Collection" component: Data Workouts table, Filter featured (limit=3), Layout Cards, Image ImageURL, Title WorkoutName_EN (temporary).

- [ ] **Step 2: Create Computed Column for Multilingual Content**
  - [ ] Go to "Data Editor", select "Workouts" table.
  - [ ] Add Computed Column "WorkoutName_Display" with formula: `IF(User Specific Columns.UserLanguage="Spanish", WorkoutName_ES, IF(User Specific Columns.UserLanguage="Haitian Creole", WorkoutName_HC, IF(User Specific Columns.UserLanguage="Chinese", WorkoutName_ZH, IF(User Specific Columns.UserLanguage="Japanese", WorkoutName_JA, WorkoutName_EN))))`
  - [ ] Add Computed Column "Description_Display" with similar formula.
  - [ ] Edit Home screen Collection component: Change title to WorkoutName_Display.

## PART 5: BUILD THE WORKOUTS SCREEN (Manual User Task)

- [ ] **Step 1: Set Up Workouts List Screen**
  - [ ] Add "Collection" component: Data Workouts table, Layout List with images, Show WorkoutName_Display, Description_Display, Duration, DifficultyLevel, Image ImageURL.

- [ ] **Step 2: Create Workout Details Screen**
  - [ ] Configure detail screen: Image (ImageURL), Title (WorkoutName_Display), Description (Description_Display), Statistics (Duration, CaloriesBurn, DifficultyLevel).
  - [ ] Add "Button" component "Start Workout": Action log to UserWorkouts, Auto-values (UserID=current user, WorkoutID=current item, DateCompleted=Today()).

## PART 6: BUILD THE SUPPLEMENTS SCREEN (Manual User Task)

- [ ] **Step 1: Set Up Supplements List**
  - [ ] Add "Collection" component: Data Supplements table, Layout List with icons, Fields SupplementName_Display (create computed column).
  - [ ] Add color-coding for safety: Conditional formatting (IF SafetyRating="Red" -> BG Red, IF "Yellow" -> BG Yellow, IF "Green" -> BG Green).

- [ ] **Step 2: Create Supplement Detail Screen**
  - [ ] Configure detail screen: Image (ImageURL), Title (SupplementName_Display), Description (Purpose_Display - create computed column), Warning section (Warnings_Display - create computed column), Contraindications section.

## PART 7: BUILD PROFILE & SETTINGS SCREENS (Manual User Task)

- [ ] **Step 1: Profile Screen**
  - [ ] Add User Profile component (Users table).
  - [ ] Add Statistics about completed workouts.
  - [ ] Add Collection of past workouts (UserWorkouts table).

- [ ] **Step 2: Settings Screen**
  - [ ] Add Language preference dropdown.
  - [ ] Add App theme settings.
  - [ ] Add Notification preferences.
  - [ ] Add About section.

## PART 8: ADD APP LOGIC & BASIC AUTOMATIONS (Manual User Task)

- [ ] **Step 1: Set Up Language Change Logic**
  - [ ] Create action "Change Language": Trigger when language dropdown changes, Action update UserLanguage column for current user.

- [ ] **Step 2: Create Sign-In Flow**
  - [ ] Enable Email sign-in.
  - [ ] Configure to save user info to Users table.

- [ ] **Step 3: Add Safety Warning Logic**
  - [ ] Create action for supplement warnings: Trigger when viewing supplement details, If SafetyRating = "Red", show warning message.

## PART 9: PUBLISH & SHARE YOUR APP (Manual User Task)

- [ ] **Step 1: Preview Your App**
  - [ ] Test all features, language switching, workout tracking, supplement info.

- [ ] **Step 2: Publish Your App**
  - [ ] Click "Publish", configure visibility Public, get URL and QR code.

## FINAL STEPS (Manual User Task)

- [ ] **Share your app.**
- [ ] **Monitor usage.**
- [ ] **Iterate.**
- [ ] **Apply for grants.**

## Additional Steps for Growth (Manual User Task)

- [ ] **Set up a Simple Analytics System.**
- [ ] **Create Social Media Presence.**
- [ ] **Prepare for Grant Applications.**

## Website Creation (Manual User Task)

- [ ] **Use Google Sites.**
- [ ] **Build Basic Site:** Logo, branding, app description, features, founder story, screenshots.
- [ ] **Add App Link:** Button to Glide app URL, QR code.

