# Ripped360 App: Data Models and User Flows (Draft 1)

This document outlines the initial draft for data models and high-level user flows for the Ripped360 application, encompassing all features as requested for simultaneous development. This is a comprehensive undertaking, and this document will be expanded iteratively.

## Part 1: Data Models

Below are the proposed data models for the core entities of the Ripped360 application. These models will form the basis of the database architecture.

### 1. User Module

**User Table (`Users`)**
*   `UserID` (Primary Key, UUID)
*   `Name` (String)
*   `Email` (String, Unique, Indexed)
*   `PasswordHash` (String)
*   `PreferredLanguage` (Enum: EN, ES, HC, ZH, JA)
*   `JoinDate` (Timestamp)
*   `FitnessGoals` (Text/JSON - e.g., weight loss, muscle gain, specific strength targets)
*   `CurrentPlanID` (Foreign Key to `TrainingPlans` - nullable)
*   `ProfileImageURL` (String, nullable)
*   `DateOfBirth` (Date, nullable)
*   `Gender` (String, nullable)
*   `Height` (Float, nullable)
*   `Weight` (Float, nullable - can be tracked over time in a separate table)
*   `StripeCustomerID` (String, nullable - for premium subscriptions)
*   `SubscriptionTier` (Enum: Free, Premium, nullable)
*   `SubscriptionExpiryDate` (Timestamp, nullable)
*   `LastLogin` (Timestamp)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**UserWeightLog Table (`UserWeightLogs`)**
*   `LogID` (Primary Key)
*   `UserID` (Foreign Key to `Users`)
*   `Date` (Timestamp)
*   `Weight` (Float)
*   `Unit` (Enum: kg, lbs)
*   `CreatedAt` (Timestamp)

**UserPreferences Table (`UserPreferences`)**
*   `PreferenceID` (Primary Key)
*   `UserID` (Foreign Key to `Users`, Unique)
*   `NotificationSettings` (JSON - e.g., workout reminders, supplement reminders, community updates)
*   `ThemePreference` (Enum: Light, Dark)
*   `MeasurementUnits` (JSON - e.g., {weight: "lbs", distance: "miles"})
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

### 2. Training Tools Module

**Exercise Table (`Exercises`)**
*   `ExerciseID` (Primary Key, UUID)
*   `ExerciseName_EN` (String)
*   `ExerciseName_ES` (String)
*   `ExerciseName_HC` (String)
*   `ExerciseName_ZH` (String)
*   `ExerciseName_JA` (String)
*   `Description_EN` (Text)
*   `Description_ES` (Text)
*   `Description_HC` (Text)
*   `Description_ZH` (Text)
*   `Description_JA` (Text)
*   `MuscleGroup` (String - e.g., Chest, Back, Legs, Biceps; could be a separate table or tags)
*   `SecondaryMuscleGroups` (Array of Strings/Tags, nullable)
*   `Equipment` (String - e.g., Barbell, Dumbbell, Bodyweight; could be a separate table or tags)
*   `DifficultyLevel` (Enum: Beginner, Intermediate, Advanced)
*   `ImageURL` (String, nullable)
*   `VideoDemonstrationURL_EN` (String, nullable)
*   `VideoDemonstrationURL_ES` (String, nullable)
*   `VideoDemonstrationURL_HC` (String, nullable)
*   `VideoDemonstrationURL_ZH` (String, nullable)
*   `VideoDemonstrationURL_JA` (String, nullable)
*   `AlternativeExerciseIDs` (Array of Foreign Keys to `Exercises`, nullable)
*   `InjuryRiskInfo_EN` (Text, nullable)
*   `InjuryRiskInfo_ES` (Text, nullable)
*   `InjuryRiskInfo_HC` (Text, nullable)
*   `InjuryRiskInfo_ZH` (Text, nullable)
*   `InjuryRiskInfo_JA` (Text, nullable)
*   `UserSubmitted` (Boolean, default: false)
*   `ValidatedByAdmin` (Boolean, default: false)
*   `CreatedByUserID` (Foreign Key to `Users`, nullable - if user-submitted)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**WorkoutLog Table (`UserWorkouts`)**
*   `LogID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `WorkoutName` (String, nullable - user-defined name for the session)
*   `DateCompleted` (Timestamp)
*   `DurationMinutes` (Integer, nullable)
*   `IntensityRating` (Integer, 1-10, nullable)
*   `Notes` (Text, nullable)
*   `ProgressNotes` (Text, nullable - for tracking progress over time for this specific workout type)
*   `CaloriesBurned` (Integer, nullable - estimated or from device)
*   `TrainingPlanID` (Foreign Key to `TrainingPlans`, nullable)
*   `WorkoutTemplateID` (Foreign Key to `WorkoutTemplates`, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**WorkoutLogExercise Table (`UserWorkoutExercises`)**
*   `LogExerciseID` (Primary Key, UUID)
*   `WorkoutLogID` (Foreign Key to `UserWorkouts`)
*   `ExerciseID` (Foreign Key to `Exercises`)
*   `OrderInWorkout` (Integer)
*   `Notes` (Text, nullable)
*   `CreatedAt` (Timestamp)

**WorkoutLogSet Table (`UserWorkoutSets`)**
*   `SetID` (Primary Key, UUID)
*   `LogExerciseID` (Foreign Key to `UserWorkoutExercises`)
*   `SetNumber` (Integer)
*   `Reps` (Integer)
*   `Weight` (Float)
*   `WeightUnit` (Enum: kg, lbs)
*   `RestPeriodSeconds` (Integer, nullable)
*   `Distance` (Float, nullable - for cardio)
*   `DistanceUnit` (Enum: km, miles, nullable)
*   `DurationSeconds` (Integer, nullable - for timed sets/cardio)
*   `HeartRate` (Integer, nullable - bpm)
*   `FormAnalysisVideoURL` (String, nullable - link to user-uploaded video for this set)
*   `AIFormFeedbackID` (Foreign Key to `AIFormFeedback`, nullable)
*   `CreatedAt` (Timestamp)

**WorkoutTemplate Table (`WorkoutTemplates`)**
*   `TemplateID` (Primary Key, UUID)
*   `TemplateName_EN` (String)
*   `TemplateName_ES` (String, nullable)
*   `TemplateName_HC` (String, nullable)
*   `TemplateName_ZH` (String, nullable)
*   `TemplateName_JA` (String, nullable)
*   `Description_EN` (Text, nullable)
*   `Description_ES` (Text, nullable)
*   `Description_HC` (Text, nullable)
*   `Description_ZH` (Text, nullable)
*   `Description_JA` (Text, nullable)
*   `CreatedByUserID` (Foreign Key to `Users`, nullable - if user-created or admin)
*   `IsPublic` (Boolean, default: false)
*   `DifficultyLevel` (Enum: Beginner, Intermediate, Advanced)
*   `Category` (String, e.g., Strength, Hypertrophy, Cardio)
*   `EstimatedDurationMinutes` (Integer, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**WorkoutTemplateExercise Table (`WorkoutTemplateExercises`)**
*   `TemplateExerciseID` (Primary Key, UUID)
*   `TemplateID` (Foreign Key to `WorkoutTemplates`)
*   `ExerciseID` (Foreign Key to `Exercises`)
*   `OrderInWorkout` (Integer)
*   `TargetSets` (Integer)
*   `TargetReps` (String - e.g., "8-10", "AMRAP")
*   `TargetWeightNotes` (String - e.g., "% of 1RM", "RPE 8")
*   `TargetRestPeriodSeconds` (Integer, nullable)
*   `Notes_EN` (Text, nullable)
*   `Notes_ES` (Text, nullable)
*   `Notes_HC` (Text, nullable)
*   `Notes_ZH` (Text, nullable)
*   `Notes_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)

**TrainingPlan Table (`TrainingPlans`)**
*   `PlanID` (Primary Key, UUID)
*   `PlanName_EN` (String)
*   `PlanName_ES` (String, nullable)
*   `PlanName_HC` (String, nullable)
*   `PlanName_ZH` (String, nullable)
*   `PlanName_JA` (String, nullable)
*   `Description_EN` (Text, nullable)
*   `Description_ES` (Text, nullable)
*   `Description_HC` (Text, nullable)
*   `Description_ZH` (Text, nullable)
*   `Description_JA` (Text, nullable)
*   `DurationWeeks` (Integer)
*   `CreatedByUserID` (Foreign Key to `Users`, nullable - if user-created or admin)
*   `IsPublic` (Boolean, default: false)
*   `AIGenerated` (Boolean, default: false)
*   `Goal` (String - e.g., Muscle Gain, Fat Loss, Strength Increase)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**TrainingPlanWorkout Table (`TrainingPlanWorkouts`)**
*   `PlanWorkoutID` (Primary Key, UUID)
*   `PlanID` (Foreign Key to `TrainingPlans`)
*   `WorkoutTemplateID` (Foreign Key to `WorkoutTemplates`)
*   `DayOfWeek` (Enum: Monday, Tuesday, etc. or DayNumber: 1, 2, ...)
*   `WeekNumber` (Integer)
*   `Notes_EN` (Text, nullable)
*   `Notes_ES` (Text, nullable)
*   `Notes_HC` (Text, nullable)
*   `Notes_ZH` (Text, nullable)
*   `Notes_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)

### 3. AI-Powered Analysis Module

**AIFormFeedback Table (`AIFormFeedback`)**
*   `FeedbackID` (Primary Key, UUID)
*   `SetID` (Foreign Key to `UserWorkoutSets`, nullable - if feedback is for a specific set)
*   `UserID` (Foreign Key to `Users`)
*   `ExerciseID` (Foreign Key to `Exercises`)
*   `VideoAnalysisInputURL` (String - link to user video or sensor data)
*   `Timestamp` (Timestamp)
*   `OverallScore` (Float, 0-100, nullable)
*   `FeedbackSummary_EN` (Text)
*   `FeedbackSummary_ES` (Text, nullable)
*   `FeedbackSummary_HC` (Text, nullable)
*   `FeedbackSummary_ZH` (Text, nullable)
*   `FeedbackSummary_JA` (Text, nullable)
*   `DetailedFeedbackPoints` (JSON - e.g., [{point: "Knee Angle", score: 70, comment_EN: "..."}, ...])
*   `CorrectiveAdvice_EN` (Text, nullable)
*   `CorrectiveAdvice_ES` (Text, nullable)
*   `CorrectiveAdvice_HC` (Text, nullable)
*   `CorrectiveAdvice_ZH` (Text, nullable)
*   `CorrectiveAdvice_JA` (Text, nullable)
*   `ProcessedByAIModelVersion` (String)
*   `CreatedAt` (Timestamp)

**AIPerformancePrediction Table (`AIPerformancePredictions`)**
*   `PredictionID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `ExerciseID` (Foreign Key to `Exercises`, nullable - if specific to an exercise)
*   `PredictionType` (Enum: StrengthGoal, WeightLoss, MuscleGain)
*   `TargetValue` (String - e.g., "100kg Bench Press", "-5kg bodyweight")
*   `PredictedCompletionDate` (Date)
*   `ConfidenceLevel` (Float, 0-1)
*   `FactorsConsidered` (JSON)
*   `GeneratedAt` (Timestamp)
*   `CreatedAt` (Timestamp)

**AIRecoveryStatus Table (`AIRecoveryStatus`)**
*   `StatusID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `Timestamp` (Timestamp)
*   `OverallRecoveryScore` (Float, 0-100)
*   `MuscleReadiness` (JSON - e.g., {chest: 80, legs: 60})
*   `NeuralFatigue` (Float, 0-100)
*   `SleepQualityScore` (Float, 0-100, nullable)
*   `Recommendations_EN` (Text, nullable)
*   `Recommendations_ES` (Text, nullable)
*   `Recommendations_HC` (Text, nullable)
*   `Recommendations_ZH` (Text, nullable)
*   `Recommendations_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)

This is the first part of the data models. I will continue with Nutrition, Supplements, Community, and other modules in subsequent updates to keep the information manageable.

Next, I will start outlining the high-level user flows for these initial modules.

---

*Self-correction: The user wants all features developed simultaneously. I should continue creating the data models for all modules before moving to user flows. I will add the remaining modules now.*



### 4. Nutrition & Health Module

**FoodItem Table (`FoodItems`)**
*   `FoodItemID` (Primary Key, UUID)
*   `FoodName_EN` (String)
*   `FoodName_ES` (String, nullable)
*   `FoodName_HC` (String, nullable)
*   `FoodName_ZH` (String, nullable)
*   `FoodName_JA` (String, nullable)
*   `BrandName` (String, nullable)
*   `Barcode` (String, Unique, Indexed, nullable)
*   `ServingSizeValue` (Float)
*   `ServingSizeUnit` (String - e.g., g, ml, oz, piece)
*   `Calories` (Float - per serving)
*   `ProteinGrams` (Float - per serving)
*   `CarbohydrateGrams` (Float - per serving)
*   `FatGrams` (Float - per serving)
*   `FiberGrams` (Float, nullable - per serving)
*   `SugarGrams` (Float, nullable - per serving)
*   `SodiumMilligrams` (Float, nullable - per serving)
*   `Micronutrients` (JSON - e.g., {vitamin_c_mg: 10, iron_mg: 2}, nullable)
*   `IsUserAdded` (Boolean, default: false)
*   `AddedByUserID` (Foreign Key to `Users`, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**UserMealLog Table (`UserMealLogs`)**
*   `MealLogID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `MealType` (Enum: Breakfast, Lunch, Dinner, Snack)
*   `LogDate` (Timestamp)
*   `Notes_EN` (Text, nullable)
*   `Notes_ES` (Text, nullable)
*   `Notes_HC` (Text, nullable)
*   `Notes_ZH` (Text, nullable)
*   `Notes_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)

**UserMealLogItem Table (`UserMealLogItems`)**
*   `MealLogItemID` (Primary Key, UUID)
*   `MealLogID` (Foreign Key to `UserMealLogs`)
*   `FoodItemID` (Foreign Key to `FoodItems`)
*   `NumberOfServings` (Float)
*   `LoggedCalories` (Float)
*   `LoggedProteinGrams` (Float)
*   `LoggedCarbohydrateGrams` (Float)
*   `LoggedFatGrams` (Float)
*   `CreatedAt` (Timestamp)

**Recipe Table (`Recipes`)**
*   `RecipeID` (Primary Key, UUID)
*   `RecipeName_EN` (String)
*   `RecipeName_ES` (String, nullable)
*   `RecipeName_HC` (String, nullable)
*   `RecipeName_ZH` (String, nullable)
*   `RecipeName_JA` (String, nullable)
*   `Description_EN` (Text, nullable)
*   `Description_ES` (Text, nullable)
*   `Description_HC` (Text, nullable)
*   `Description_ZH` (Text, nullable)
*   `Description_JA` (Text, nullable)
*   `Instructions_EN` (Text)
*   `Instructions_ES` (Text, nullable)
*   `Instructions_HC` (Text, nullable)
*   `Instructions_ZH` (Text, nullable)
*   `Instructions_JA` (Text, nullable)
*   `PrepTimeMinutes` (Integer, nullable)
*   `CookTimeMinutes` (Integer, nullable)
*   `Servings` (Integer)
*   `ImageURL` (String, nullable)
*   `CulturalAdaptationNotes` (JSON - e.g., {ES: "Substitute X for Y for Latin American palate"})
*   `CreatedByUserID` (Foreign Key to `Users`, nullable - if user-submitted or admin)
*   `IsPublic` (Boolean, default: true)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**RecipeIngredient Table (`RecipeIngredients`)**
*   `RecipeIngredientID` (Primary Key, UUID)
*   `RecipeID` (Foreign Key to `Recipes`)
*   `FoodItemID` (Foreign Key to `FoodItems`, nullable - if using a standard food item)
*   `IngredientName_EN` (String - if not a standard food item)
*   `IngredientName_ES` (String, nullable)
*   `IngredientName_HC` (String, nullable)
*   `IngredientName_ZH` (String, nullable)
*   `IngredientName_JA` (String, nullable)
*   `Quantity` (Float)
*   `Unit` (String - e.g., g, cup, tbsp)
*   `Notes_EN` (Text, nullable)
*   `Notes_ES` (Text, nullable)
*   `Notes_HC` (Text, nullable)
*   `Notes_ZH` (Text, nullable)
*   `Notes_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)

**UserDailyNutritionSummary Table (`UserDailyNutritionSummaries`)**
*   `SummaryID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `Date` (Date)
*   `TotalCaloriesConsumed` (Float)
*   `TotalProteinGrams` (Float)
*   `TotalCarbohydrateGrams` (Float)
*   `TotalFatGrams` (Float)
*   `TargetCalories` (Float, nullable - from user goals or plan)
*   `TargetProteinGrams` (Float, nullable)
*   `TargetCarbohydrateGrams` (Float, nullable)
*   `TargetFatGrams` (Float, nullable)
*   `WorkoutAdjustedCalories` (Float, nullable - if synced with workout)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

### 5. Supplement & Medication Module

**Supplement Table (`Supplements`)**
*   `SupplementID` (Primary Key, UUID)
*   `SupplementName_EN` (String)
*   `SupplementName_ES` (String, nullable)
*   `SupplementName_HC` (String, nullable)
*   `SupplementName_ZH` (String, nullable)
*   `SupplementName_JA` (String, nullable)
*   `BrandName` (String, nullable)
*   `Category` (String - e.g., Protein, Creatine, Vitamin)
*   `Purpose_EN` (Text, nullable)
*   `Purpose_ES` (Text, nullable)
*   `Purpose_HC` (Text, nullable)
*   `Purpose_ZH` (Text, nullable)
*   `Purpose_JA` (Text, nullable)
*   `Ingredients` (JSON - e.g., [{name: "Creatine Monohydrate", amount: "5g"}])
*   `ServingSizeInfo` (String)
*   `ImageURL` (String, nullable)
*   `Barcode` (String, Unique, Indexed, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**Medication Table (`Medications`)**
*   `MedicationID` (Primary Key, UUID)
*   `MedicationName` (String)
*   `GenericName` (String, nullable)
*   `DosageForm` (String - e.g., Tablet, Capsule, Liquid)
*   `Strength` (String - e.g., "500mg", "10mg/mL")
*   `Purpose_EN` (Text, nullable)
*   `Purpose_ES` (Text, nullable)
*   `Purpose_HC` (Text, nullable)
*   `Purpose_ZH` (Text, nullable)
*   `Purpose_JA` (Text, nullable)
*   `Barcode` (String, Unique, Indexed, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**UserSupplementLog Table (`UserSupplementLogs`)**
*   `LogID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `SupplementID` (Foreign Key to `Supplements`)
*   `DosageAmount` (String)
*   `IntakeTimestamp` (Timestamp)
*   `Notes_EN` (Text, nullable)
*   `Notes_ES` (Text, nullable)
*   `Notes_HC` (Text, nullable)
*   `Notes_ZH` (Text, nullable)
*   `Notes_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)

**UserMedicationLog Table (`UserMedicationLogs`)**
*   `LogID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `MedicationID` (Foreign Key to `Medications`)
*   `DosageAmount` (String)
*   `IntakeTimestamp` (Timestamp)
*   `PrescribedBy` (String, nullable)
*   `Notes_EN` (Text, nullable)
*   `Notes_ES` (Text, nullable)
*   `Notes_HC` (Text, nullable)
*   `Notes_ZH` (Text, nullable)
*   `Notes_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)

**InteractionWarning Table (`InteractionWarnings`)**
*   `WarningID` (Primary Key, UUID)
*   `Item1Type` (Enum: Supplement, Medication, FoodCategory)
*   `Item1ID` (UUID, Foreign Key to `Supplements` or `Medications` or a new `FoodCategories` table)
*   `Item2Type` (Enum: Supplement, Medication, FoodCategory)
*   `Item2ID` (UUID, Foreign Key to `Supplements` or `Medications` or `FoodCategories`)
*   `Severity` (Enum: Dangerous, Caution, Beneficial, NoInteraction)
*   `Description_EN` (Text)
*   `Description_ES` (Text, nullable)
*   `Description_HC` (Text, nullable)
*   `Description_ZH` (Text, nullable)
*   `Description_JA` (Text, nullable)
*   `DataSource` (String - e.g., FDA, ResearchPaperID)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**Reminder Table (`Reminders`)**
*   `ReminderID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `ItemType` (Enum: Supplement, Medication, Workout, Meal)
*   `ItemID` (UUID, Foreign Key to respective item table, nullable)
*   `ReminderTime` (Time)
*   `Frequency` (String - e.g., Daily, SpecificDaysOfWeek, Once)
*   `Message_EN` (String)
*   `Message_ES` (String, nullable)
*   `Message_HC` (String, nullable)
*   `Message_ZH` (String, nullable)
*   `Message_JA` (String, nullable)
*   `IsEnabled` (Boolean, default: true)
*   `SnoozeCount` (Integer, default: 0)
*   `LastTriggeredAt` (Timestamp, nullable)
*   `SupplyLowWarning` (Boolean, default: false - for supplements/meds)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

### 6. Community & Integration Module

**CommunityChallenge Table (`CommunityChallenges`)**
*   `ChallengeID` (Primary Key, UUID)
*   `ChallengeName_EN` (String)
*   `ChallengeName_ES` (String, nullable)
*   `ChallengeName_HC` (String, nullable)
*   `ChallengeName_ZH` (String, nullable)
*   `ChallengeName_JA` (String, nullable)
*   `Description_EN` (Text)
*   `Description_ES` (Text, nullable)
*   `Description_HC` (Text, nullable)
*   `Description_ZH` (Text, nullable)
*   `Description_JA` (Text, nullable)
*   `StartDate` (Timestamp)
*   `EndDate` (Timestamp)
*   `GoalType` (Enum: TotalWorkouts, TotalVolume, SpecificExercisePR, etc.)
*   `GoalValue` (String)
*   `Reward_EN` (String, nullable)
*   `Reward_ES` (String, nullable)
*   `Reward_HC` (String, nullable)
*   `Reward_ZH` (String, nullable)
*   `Reward_JA` (String, nullable)
*   `CreatedByAdminID` (Foreign Key to `Users` - admin user)
*   `IsActive` (Boolean, default: true)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**UserChallengeParticipation Table (`UserChallengeParticipations`)**
*   `ParticipationID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `ChallengeID` (Foreign Key to `CommunityChallenges`)
*   `JoinDate` (Timestamp)
*   `ProgressValue` (String)
*   `Rank` (Integer, nullable)
*   `IsCompleted` (Boolean, default: false)
*   `CompletionDate` (Timestamp, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**UserPost Table (`UserPosts`)**
*   `PostID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `Content_EN` (Text)
*   `Content_ES` (Text, nullable)
*   `Content_HC` (Text, nullable)
*   `Content_ZH` (Text, nullable)
*   `Content_JA` (Text, nullable)
*   `ImageURL` (String, nullable)
*   `VideoURL` (String, nullable)
*   `Visibility` (Enum: Public, Followers, Private)
*   `RelatedWorkoutLogID` (Foreign Key to `UserWorkouts`, nullable)
*   `RelatedChallengeID` (Foreign Key to `CommunityChallenges`, nullable)
*   `Tags` (Array of Strings, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**PostLike Table (`PostLikes`)**
*   `LikeID` (Primary Key, UUID)
*   `PostID` (Foreign Key to `UserPosts`)
*   `UserID` (Foreign Key to `Users`)
*   `CreatedAt` (Timestamp)
*   (Composite unique key on PostID, UserID)

**PostComment Table (`PostComments`)**
*   `CommentID` (Primary Key, UUID)
*   `PostID` (Foreign Key to `UserPosts`)
*   `UserID` (Foreign Key to `Users`)
*   `ParentCommentID` (Foreign Key to `PostComments`, nullable - for threaded comments)
*   `Content_EN` (Text)
*   `Content_ES` (Text, nullable)
*   `Content_HC` (Text, nullable)
*   `Content_ZH` (Text, nullable)
*   `Content_JA` (Text, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**UserFollow Table (`UserFollows`)**
*   `FollowID` (Primary Key, UUID)
*   `FollowerUserID` (Foreign Key to `Users`)
*   `FollowingUserID` (Foreign Key to `Users`)
*   `CreatedAt` (Timestamp)
*   (Composite unique key on FollowerUserID, FollowingUserID)

**DeviceIntegration Table (`DeviceIntegrations`)**
*   `IntegrationID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `PlatformName` (Enum: AppleHealth, GoogleFit, Fitbit, Garmin, InBody, etc.)
*   `AccessToken` (String, Encrypted)
*   `RefreshToken` (String, Encrypted, nullable)
*   `TokenExpiry` (Timestamp, nullable)
*   `LastSyncTimestamp` (Timestamp, nullable)
*   `IsEnabled` (Boolean, default: true)
*   `SyncPreferences` (JSON - e.g., {workouts: true, heartRate: true, steps: false})
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**CalendarIntegration Table (`CalendarIntegrations`)**
*   `IntegrationID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `CalendarProvider` (Enum: GoogleCalendar, OutlookCalendar)
*   `AccessToken` (String, Encrypted)
*   `RefreshToken` (String, Encrypted, nullable)
*   `CalendarIDUsed` (String)
*   `IsEnabled` (Boolean, default: true)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

### 7. Brand Integration & Educational Resources Module

**RippedCityProduct Table (`RippedCityProducts`)**
*   `ProductID` (Primary Key, UUID)
*   `ProductName_EN` (String)
*   `ProductName_ES` (String, nullable)
*   `ProductName_HC` (String, nullable)
*   `ProductName_ZH` (String, nullable)
*   `ProductName_JA` (String, nullable)
*   `Description_EN` (Text, nullable)
*   `Description_ES` (Text, nullable)
*   `Description_HC` (Text, nullable)
*   `Description_ZH` (Text, nullable)
*   `Description_JA` (Text, nullable)
*   `ProductType` (Enum: Apparel, Accessory)
*   `ImageURL` (String)
*   `Price` (Decimal)
*   `Currency` (String, default: USD)
*   `LinkToStore` (String)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**PersonalizedProductSuggestion Table (`PersonalizedProductSuggestions`)**
*   `SuggestionID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `ProductID` (Foreign Key to `RippedCityProducts`)
*   `Reasoning_EN` (Text, nullable - e.g., "Based on your workout focus on X")
*   `Reasoning_ES` (Text, nullable)
*   `Reasoning_HC` (Text, nullable)
*   `Reasoning_ZH` (Text, nullable)
*   `Reasoning_JA` (Text, nullable)
*   `GeneratedAt` (Timestamp)
*   `IsClicked` (Boolean, default: false)
*   `IsPurchased` (Boolean, default: false)
*   `CreatedAt` (Timestamp)

**EducationalResource Table (`EducationalResources`)**
*   `ResourceID` (Primary Key, UUID)
*   `Title_EN` (String)
*   `Title_ES` (String, nullable)
*   `Title_HC` (String, nullable)
*   `Title_ZH` (String, nullable)
*   `Title_JA` (String, nullable)
*   `Content_EN` (Text)
*   `Content_ES` (Text, nullable)
*   `Content_HC` (Text, nullable)
*   `Content_ZH` (Text, nullable)
*   `Content_JA` (Text, nullable)
*   `ResourceType` (Enum: Article, Video, InteractiveModule, AnatomyGuide, ExerciseScienceFact)
*   `Category` (String - e.g., Nutrition, StrengthTraining, InjuryPrevention)
*   `Author` (String, nullable)
*   `ImageURL` (String, nullable)
*   `VideoURL` (String, nullable)
*   `InteractiveModuleURL` (String, nullable)
*   `PublishedAt` (Timestamp)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

**UserEducationalProgress Table (`UserEducationalProgress`)**
*   `ProgressID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `ResourceID` (Foreign Key to `EducationalResources`)
*   `Status` (Enum: NotStarted, InProgress, Completed)
*   `CompletionDate` (Timestamp, nullable)
*   `LastAccessedAt` (Timestamp)
*   `UserRating` (Integer, 1-5, nullable)
*   `CreatedAt` (Timestamp)
*   `UpdatedAt` (Timestamp)

### 8. Weather & Seasonal Activity Module

**WeatherPredictionLog Table (`WeatherPredictionLogs`)**
*   `LogID` (Primary Key, UUID)
*   `UserID` (Foreign Key to `Users`)
*   `LocationCoordinates` (Point - lat/long)
*   `Timestamp` (Timestamp)
*   `WeatherData` (JSON - from weather API)
*   `SuggestedActivity_EN` (String, nullable)
*   `SuggestedActivity_ES` (String, nullable)
*   `SuggestedActivity_HC` (String, nullable)
*   `SuggestedActivity_ZH` (String, nullable)
*   `SuggestedActivity_JA` (String, nullable)
*   `OptimalWorkoutTimeSuggestion_EN` (String, nullable)
*   `OptimalWorkoutTimeSuggestion_ES` (String, nullable)
*   `OptimalWorkoutTimeSuggestion_HC` (String, nullable)
*   `OptimalWorkoutTimeSuggestion_ZH` (String, nullable)
*   `OptimalWorkoutTimeSuggestion_JA` (String, nullable)
*   `CreatedAt` (Timestamp)

This completes the initial draft of the data models for all major modules. Next, I will begin outlining the high-level user flows for these modules.




## Part 2: High-Level User Flows

This section outlines high-level user flows for the Ripped360 application. These flows describe the primary paths users will take to interact with the app's features.

### 1. User Module Flows

**1.1. User Registration (New User)**
1.  User opens app/website.
2.  User selects "Sign Up" option.
3.  System displays registration form (Name, Email, Password, Confirm Password, Preferred Language).
4.  User fills in the form and submits.
5.  System validates input:
    *   If invalid (e.g., email format, password mismatch, email already exists): Display error message.
    *   If valid: Create new `User` record. Hash password. Store `PreferredLanguage`.
6.  System logs the user in automatically.
7.  System navigates user to the main dashboard/home screen.
    *   *Optional: System may prompt for initial profile setup (DOB, Gender, Height, Weight, Fitness Goals).* 

**1.2. User Login (Existing User)**
1.  User opens app/website.
2.  User selects "Log In" option.
3.  System displays login form (Email, Password).
4.  User fills in the form and submits.
5.  System validates credentials:
    *   If invalid (e.g., email not found, password incorrect): Display error message.
    *   If valid: Update `LastLogin` timestamp for the user.
6.  System navigates user to the main dashboard/home screen.

**1.3. User Logout**
1.  User navigates to profile/settings section.
2.  User selects "Log Out" option.
3.  System clears user session/token.
4.  System navigates user to the login/landing screen.

**1.4. Edit User Profile**
1.  User navigates to profile screen.
2.  User selects "Edit Profile" option.
3.  System displays editable fields (Name, Email, Profile Image, DOB, Gender, Height, Weight, Fitness Goals, Preferred Language).
4.  User modifies desired fields and saves changes.
5.  System validates input.
6.  System updates the `User` record and related records (e.g., `UserWeightLogs` if weight is updated).
7.  System displays confirmation message.

**1.5. Change Password**
1.  User navigates to settings/security section.
2.  User selects "Change Password" option.
3.  System displays form (Current Password, New Password, Confirm New Password).
4.  User fills in the form and submits.
5.  System validates current password and new password rules:
    *   If invalid: Display error message.
    *   If valid: Update `PasswordHash` in `Users` table.
6.  System displays confirmation message.
    *   *Optional: Log out user from other active sessions.* 

**1.6. Set/Update Preferences**
1.  User navigates to settings screen.
2.  User selects "Preferences" (e.g., Notifications, Theme, Measurement Units).
3.  System displays preference options.
4.  User modifies preferences and saves.
5.  System updates `UserPreferences` record.
6.  System applies changes immediately (e.g., theme change).

### 2. Training Tools Module Flows

**2.1. Browse Exercise Library**
1.  User navigates to "Exercises" or "Library" section.
2.  System displays a list/grid of exercises, potentially with filters (muscle group, equipment, difficulty) and search functionality.
3.  User selects an exercise.
4.  System displays detailed exercise information: Name (in user's preferred language), Description, Muscle Group, Equipment, Difficulty, Image/Video Demonstration, Injury Risk Info, Alternative Exercises.
5.  User can navigate back to the list or select another exercise.

**2.2. Log a Workout (Manual Entry)**
1.  User navigates to "Log Workout" or a similar section (e.g., from a calendar or dashboard).
2.  User chooses to start a new blank workout (or select a template - see 2.3).
3.  System creates a new `UserWorkouts` record (initially with date, UserID).
4.  User gives the workout a name (optional).
5.  User adds exercises to the workout:
    *   User searches/selects an exercise from the library.
    *   For the selected exercise, user adds sets:
        *   User inputs Reps, Weight (and unit).
        *   User can add multiple sets for the exercise.
        *   System saves each set to `UserWorkoutSets` linked to `UserWorkoutExercises`.
    *   User can add more exercises.
6.  During or after the workout, user can input overall workout duration, intensity rating, notes.
7.  User selects "Finish Workout" or "Save Workout".
8.  System finalizes the `UserWorkouts` record and associated `UserWorkoutExercises` and `UserWorkoutSets`.
9.  System displays a summary of the logged workout.

**2.3. Log a Workout using a Workout Template**
1.  User navigates to "Log Workout" or "Start Workout from Template".
2.  User browses/selects a `WorkoutTemplate`.
3.  System pre-populates a new workout log based on the template's exercises, target sets, reps, etc.
4.  User performs the workout, adjusting actual reps/weight for each set as needed.
5.  User can add/remove exercises or sets if deviating from the template.
6.  User selects "Finish Workout".
7.  System saves the workout log (as in 2.2), potentially linking it to the `WorkoutTemplateID`.

**2.4. View Workout History**
1.  User navigates to "Workout History" or "Progress" section.
2.  System displays a list of past `UserWorkouts`, sortable/filterable by date, workout name, etc.
3.  User selects a specific workout log.
4.  System displays detailed information for the selected workout: Date, Name, Duration, Exercises performed, Sets/Reps/Weight for each exercise, Notes.

**2.5. Create/Manage Workout Templates (User-Created)**
1.  User navigates to "My Templates" or "Workout Templates" section.
2.  User selects "Create New Template".
3.  User names the template and adds a description (multilingual fields available).
4.  User adds exercises to the template (from library) and defines target sets, reps, rest periods for each.
5.  User saves the `WorkoutTemplate` and associated `WorkoutTemplateExercises`.
6.  User can later view, edit, or delete their created templates.

**2.6. Use a Training Plan**
1.  User browses available `TrainingPlans` (public or AI-generated for them).
2.  User selects a training plan and views its details (duration, workouts per week, description).
3.  User chooses to "Start this Plan".
4.  System assigns the `TrainingPlanID` to the user (e.g., in `Users.CurrentPlanID`).
5.  On scheduled days (or when user navigates to "Today's Workout"), system prompts user with the workout from the plan (based on `TrainingPlanWorkouts` which links to `WorkoutTemplates`).
6.  User logs the workout (as in 2.3).
7.  System tracks user's progress through the plan.

I will continue with user flows for AI-Powered Analysis, Nutrition & Health, Supplement & Medication, Community & Integration, Brand Integration, and Weather modules in the next update to this file.

### 3. AI-Powered Analysis Module Flows



**3.1. Submit Video for Form Analysis**
1.  During or after logging a set (`UserWorkoutSets`), user sees an option "Upload video for Form Analysis".
2.  User selects the option and uploads a video file from their device.
3.  System validates video file format and size.
    *   If invalid: Display error message.
    *   If valid: Upload video to secure storage. Create/update `UserWorkoutSets.FormAnalysisVideoURL`.
4.  System queues the video for AI processing (asynchronous).
5.  User is notified that the analysis is in progress and they will be alerted when complete.

**3.2. View Form Analysis Feedback**
1.  User receives a notification (in-app or push) that form analysis is complete OR user navigates to a workout log where a video was submitted.
2.  User selects to view feedback for a specific analyzed set/video.
3.  System retrieves the `AIFormFeedback` record associated with the `UserWorkoutSets` or video.
4.  System displays: Overall Score, Feedback Summary (in user's language), Detailed Feedback Points (e.g., joint angles, movement paths, highlighted on video frames if possible), Corrective Advice.

**3.3. View Performance Predictions**
1.  User navigates to "Progress" or "AI Insights" section.
2.  System displays any available `AIPerformancePredictions` for the user (e.g., predicted 1RM, time to reach a goal).
3.  User can view details of a prediction, including factors considered and confidence level.

**3.4. View AI-Generated Training Plan (if applicable)**
1.  If a user's `TrainingPlan` was AI-generated, they can view the rationale or adaptive changes made by the AI.
2.  System displays insights on why certain exercises or structures were chosen/modified based on user's progress and recovery.

**3.5. View Recovery Status**
1.  User navigates to dashboard or "Recovery" section.
2.  System displays the latest `AIRecoveryStatus` for the user: Overall Score, Muscle Readiness (e.g., per body part), Neural Fatigue, Sleep Quality (if data available).
3.  System displays AI recommendations based on recovery status (e.g., "Focus on mobility today", "Reduce intensity for leg exercises").

### 4. Nutrition & Health Module Flows

**4.1. Log Food Item (Manual Entry/Search)**
1.  User navigates to "Nutrition Log" or "Add Meal".
2.  User selects a meal type (Breakfast, Lunch, Dinner, Snack).
3.  User searches for a food item in the `FoodItems` database.
4.  System displays search results (Food Name, Brand, Serving Size).
5.  User selects a food item.
6.  User inputs the number of servings consumed.
7.  System calculates and displays nutritional info for the entered amount (Calories, Protein, Carbs, Fat).
8.  User confirms to add the item to the current `UserMealLog`.
9.  System creates/updates `UserMealLogItems` and updates `UserDailyNutritionSummaries`.
    *   *If food not found:* User may have an option to "Add New Food Item" (see 4.2).

**4.2. Add New Food Item to Database (User-Submitted)**
1.  From food logging flow, if an item is not found, user selects "Add New Food".
2.  System displays a form for Food Name, Brand (optional), Serving Size, Calories, Protein, Carbs, Fat, etc.
3.  User fills in the details and saves.
4.  System creates a new `FoodItems` record with `IsUserAdded` = true and `AddedByUserID`.
5.  User can then log this newly added item.

**4.3. Log Food Item (Barcode Scan)**
1.  User navigates to "Nutrition Log" or "Add Meal".
2.  User selects "Scan Barcode" option.
3.  System activates device camera for scanning.
4.  User scans a food product barcode.
5.  System searches `FoodItems.Barcode`:
    *   If found: System pre-fills the food item details. User inputs number of servings and confirms (as in 4.1).
    *   If not found: System informs user. User may be prompted to add it manually (as in 4.2), including the scanned barcode.

**4.4. View Daily Nutrition Summary**
1.  User navigates to "Nutrition" dashboard or "Daily Summary".
2.  System displays `UserDailyNutritionSummaries` for the current day (or selected day): Total Calories, Protein, Carbs, Fat consumed vs. targets.
3.  System may show a breakdown by meal or individual food items logged.

**4.5. Use a Meal Plan/Recipe**
1.  User navigates to "Meal Plans" or "Recipes" section.
2.  User browses/searches for `Recipes` (can be filtered by cultural adaptation, dietary needs).
3.  User selects a recipe and views details: Ingredients, Instructions, Nutritional Info (calculated).
4.  User can choose to "Log this Meal" (which pre-fills food items into a meal log) or "Add to My Meal Plan".

**4.6. AI Meal Recommendations**
1.  After a workout, or based on daily nutrition summary, system may proactively provide AI meal recommendations.
2.  E.g., "Based on today's leg workout, add 25g extra protein. Consider this recipe: [Link to Recipe]".
3.  These recommendations are based on user's goals, activity, and nutritional needs.

### 5. Supplement & Medication Module Flows

**5.1. Log Supplement Intake**
1.  User navigates to "Supplements" or "Log Intake".
2.  User searches/selects a `Supplement` from their list or the main database.
3.  User inputs dosage amount and confirms intake time (defaults to now).
4.  System creates a `UserSupplementLog` record.
5.  System checks for potential interactions with other logged supplements/medications based on `InteractionWarnings` and alerts user if necessary.

**5.2. Log Medication Intake**
1.  Similar to 5.1, but for `Medications`.
2.  User searches/selects a `Medication`.
3.  User inputs dosage and confirms intake time.
4.  System creates a `UserMedicationLog` record.
5.  System checks for interactions and alerts user.

**5.3. Add New Supplement/Medication to User's List (or Database if user-added allowed)**
1.  If a supplement/medication is not found, user can add it.
2.  User inputs Name, Brand (for supplements), Dosage info, etc.
3.  System creates a new `Supplements` or `Medications` record (if global contribution allowed and moderated) or links to a user-specific list of custom items.
4.  User can then log intake for this new item.

**5.4. Scan Supplement/Medication Barcode**
1.  User selects "Scan Barcode" in supplement/medication logging.
2.  User scans product barcode.
3.  System searches `Supplements.Barcode` or `Medications.Barcode`.
    *   If found: Pre-fills item details for logging.
    *   If not found: Prompts user to add manually.

**5.5. Set/Manage Reminders**
1.  User navigates to "Reminders" section.
2.  User creates a new reminder: Selects item type (Supplement, Medication, Workout, Meal), selects specific item (if applicable), sets time, frequency, and custom message (multilingual).
3.  System saves `Reminder` record.
4.  System triggers notifications based on reminder settings.
5.  User can view, edit, or disable existing reminders.
6.  System may automatically create reminders (e.g., for low supply based on logging frequency).

**5.6. View Interaction Warnings**
1.  When logging a supplement/medication, system automatically checks and displays warnings.
2.  User can also access a dedicated section to check potential interactions between two or more items they are taking/considering.

**5.7. View Active Duration Visualization (Conceptual)**
1.  User views a timeline showing estimated active durations of logged supplements/medications.
2.  This helps optimize timing around workouts or other medications.

### 6. Community & Integration Module Flows

**6.1. Participate in a Community Challenge**
1.  User navigates to "Challenges" section.
2.  System displays active `CommunityChallenges`.
3.  User selects a challenge to view details (goal, duration, participants, leaderboard).
4.  User joins the challenge. System creates `UserChallengeParticipation` record.
5.  User's relevant activities (e.g., logged workouts) automatically contribute to their challenge progress.
6.  User can view their progress and rank on the leaderboard.

**6.2. Share Progress/Achievements (Create Post)**
1.  User achieves a milestone (e.g., new PR, completed workout, challenge goal).
2.  System may prompt user to share, or user navigates to "Create Post".
3.  User writes content (multilingual), can attach image/video, link related workout/challenge.
4.  User sets visibility and posts. System creates `UserPost` record.

**6.3. Interact with Community Feed (View Posts, Like, Comment)**
1.  User navigates to community feed.
2.  System displays `UserPosts` from followed users or public posts.
3.  User can like a post (creates `PostLike` record).
4.  User can comment on a post (creates `PostComment` record). Can reply to comments.

**6.4. Follow/Unfollow Users**
1.  User views another user's profile.
2.  User selects "Follow". System creates `UserFollow` record.
3.  User can later unfollow.

**6.5. Connect/Manage Device Integrations**
1.  User navigates to Settings -> "Connected Devices/Apps".
2.  User selects a platform (e.g., Apple Health, Google Fit).
3.  System initiates OAuth flow for the selected platform.
4.  User authorizes Ripped360 to access data.
5.  System stores tokens and creates `DeviceIntegration` record.
6.  System periodically syncs data based on user preferences.
7.  User can manage sync preferences or disconnect devices.

**6.6. Connect/Manage Calendar Integrations**
1.  Similar to 6.5, but for calendar providers (Google Calendar, Outlook).
2.  User authorizes access.
3.  System can then schedule workouts/reminders in user's external calendar.

### 7. Brand Integration & Educational Resources Module Flows

**7.1. View Personalized Product Suggestions**
1.  User is on dashboard or specific sections (e.g., after a workout type that matches an apparel category).
2.  System displays `PersonalizedProductSuggestions` (RippedCity apparel/accessories).
3.  User can click a suggestion to view product details (name, image, price) and link to store.

**7.2. Browse/Access Educational Resources**
1.  User navigates to "Learn" or "Resources" section.
2.  System displays `EducationalResources` (articles, videos, etc.), filterable by category/type.
3.  User selects a resource to view/read/watch content (in preferred language).
4.  System tracks progress in `UserEducationalProgress`.
5.  User can rate resources.

### 8. Weather & Seasonal Activity Module Flows

**8.1. View Weather-Based Activity Suggestions**
1.  User is on dashboard or planning a workout.
2.  If location services are enabled, system fetches current/forecasted weather for user's location.
3.  System displays `WeatherPredictionLog` data, including suggested activities suitable for the weather (e.g., "Indoor workout recommended due to rain") and optimal workout times.

This completes the initial draft of high-level user flows for all major modules. These will be refined and detailed further in the next stages of design and development.

