# Data Flow & Structure

This document explains the transfer of data between front-end and back-end components, showing a clear flow of information.

---

## 1. USER SUBMISSION TO BACK-END API

User submits their startup idea through the web form, which is sent to the back-end for processing.

**Route**: `POST /api/validate`

**Request Payload**
```json
{
  "title": "AI-Powered Fitness Tracker",
  "description": "A mobile app that uses computer vision to track workout form and provide real-time feedback. Features include real-time form analysis, personalized workout plans, progress tracking, and social sharing capabilities."
}
```

---

## 2. BACK-END DATABASE QUERY

The back-end queries MongoDB using vector embeddings to find startups with similar descriptions and features.

**Vector Search Results**
```json
{
  "similar_startups": [
    {
      "id": "startup_001",
      "name": "FormCheck AI",
      "description": "AI-powered workout form correction app with form tracking, AI coaching, and video analysis",
      "funding": 2500000,
      "customer_size": 50000
    },
    {
      "id": "startup_002",
      "name": "FitVision",
      "description": "Computer vision fitness assistant with exercise recognition, rep counting, and form feedback",
      "funding": 1800000,
      "customer_size": 30000
    },
    {
      "id": "startup_003",
      "name": "GymAI",
      "description": "Smart fitness tracking with AI analysis, workout tracking, AI recommendations, and progress analytics",
      "funding": 3200000,
      "customer_size": 100000
    }
  ]
}
```

## 3. BACK-END TO GEMINI AI

The back-end combines the user's startup idea with competitor data from MongoDB and sends it to Gemini for AI-powered analysis.

**Analysis Request Payload**
```json
{
  "user_idea": {
    "title": "AI-Powered Fitness Tracker",
    "description": "A mobile app that uses computer vision to track workout form and provide real-time feedback. Features include real-time form analysis, personalized workout plans, progress tracking, and social sharing capabilities."
  },
  "similar_startups": [
    {
      "name": "FormCheck AI",
      "description": "AI-powered workout form correction app with form tracking, AI coaching, and video analysis",
      "funding": 2500000,
      "customer_size": 50000
    },
    {
      "name": "FitVision",
      "description": "Computer vision fitness assistant with exercise recognition, rep counting, and form feedback",
      "funding": 1800000,
      "customer_size": 30000
    },
    {
      "name": "GymAI",
      "description": "Smart fitness tracking with AI analysis, workout tracking, AI recommendations, and progress analytics",
      "funding": 3200000,
      "customer_size": 100000
    }
  ]
}
```

## 4. GEMINI AI TO BACK-END

Gemini processes the data and returns a detailed analysis including competitor insights, feature metrics, market gaps, suggestions, and success likelihood.

**Analysis Response**
```json
{
  "competitor_analysis": {
    "direct_competitors": [
      {
        "name": "FormCheck AI",
        "strengths": ["Established user base", "Strong AI technology"],
        "weaknesses": ["Limited social features", "High price point"]
      },
      {
        "name": "FitVision",
        "strengths": ["Accurate computer vision", "User-friendly interface"],
        "weaknesses": ["No personalized plans", "Basic analytics"]
      }
    ],
    "indirect_competitors": [
      {
        "name": "GymAI",
        "overlap": "Fitness tracking and AI recommendations"
      }
    ]
  },
  "feature_metrics": {
    "ai_technology": 85,
    "user_experience": 70,
    "social_features": 60,
    "personalization": 75,
    "analytics": 65
  },
  "market_gaps": [
    "Real-time social interaction during workouts",
    "Integration with wearable devices",
    "Gamification elements for motivation",
    "Affordable pricing tier for beginners"
  ],
  "suggestions": [
    {
      "marked_text": "*computer vision*",
      "suggestion": "Consider expanding to include motion sensor integration for more accurate tracking",
      "rationale": "Current competitors rely heavily on camera-only input which has limitations in certain lighting conditions"
    },
    {
      "marked_text": "*real-time feedback*",
      "suggestion": "Add haptic feedback integration for immediate corrections",
      "rationale": "Users responding to visual cues alone miss 40% of form corrections during intense workouts"
    },
    {
      "marked_text": "*social sharing capabilities*",
      "suggestion": "Pivot to live workout sessions with friends rather than post-workout sharing",
      "rationale": "Real-time social interaction is an identified market gap with low competition"
    }
  ],
  "pivot_suggestions": [
    {
      "suggestion": "Target corporate wellness programs",
      "rationale": "Underserved B2B market with higher revenue potential"
    },
    {
      "suggestion": "Specialize in specific sports like weightlifting only",
      "rationale": "Niche focus allows for deeper expertise and less competition"
    }
  ],
  "likelihood_of_success": 72
}
```

---

## 5. BACK-END API TO FRONT-END

The back-end packages the user's idea (with suggestion markers), similar startups, and complete analysis into a single response sent to the front-end.

**Route**: `POST /api/validate` Response

**Response Payload**
```json
{
  "user_idea": {
    "title": "AI-Powered Fitness Tracker",
    "description": "A mobile app that uses *computer vision* to track workout form and provide *real-time feedback*. Features include real-time form analysis, personalized workout plans, progress tracking, and *social sharing capabilities*."
  },
  "similar_startups": [
    {
      "id": "startup_001",
      "name": "FormCheck AI",
      "description": "AI-powered workout form correction app with form tracking, AI coaching, and video analysis",
      "funding": 2500000,
      "customer_size": 50000
    },
    {
      "id": "startup_002",
      "name": "FitVision",
      "description": "Computer vision fitness assistant with exercise recognition, rep counting, and form feedback",
      "funding": 1800000,
      "customer_size": 30000
    },
    {
      "id": "startup_003",
      "name": "GymAI",
      "description": "Smart fitness tracking with AI analysis, workout tracking, AI recommendations, and progress analytics",
      "funding": 3200000,
      "customer_size": 100000
    }
  ],
  "analysis": {
    "competitor_analysis": {
      "direct_competitors": [
        {
          "name": "FormCheck AI",
          "strengths": ["Established user base", "Strong AI technology"],
          "weaknesses": ["Limited social features", "High price point"]
        },
        {
          "name": "FitVision",
          "strengths": ["Accurate computer vision", "User-friendly interface"],
          "weaknesses": ["No personalized plans", "Basic analytics"]
        }
      ],
      "indirect_competitors": [
        {
          "name": "GymAI",
          "overlap": "Fitness tracking and AI recommendations"
        }
      ]
    },
    "feature_metrics": {
      "ai_technology": 85,
      "user_experience": 70,
      "social_features": 60,
      "personalization": 75,
      "analytics": 65
    },
    "market_gaps": [
      "Real-time social interaction during workouts",
      "Integration with wearable devices",
      "Gamification elements for motivation",
      "Affordable pricing tier for beginners"
    ],
    "suggestions": [
      {
        "marked_text": "*computer vision*",
        "suggestion": "Consider expanding to include motion sensor integration for more accurate tracking",
        "rationale": "Current competitors rely heavily on camera-only input which has limitations in certain lighting conditions"
      },
      {
        "marked_text": "*real-time feedback*",
        "suggestion": "Add haptic feedback integration for immediate corrections",
        "rationale": "Users responding to visual cues alone miss 40% of form corrections during intense workouts"
      },
      {
        "marked_text": "*social sharing capabilities*",
        "suggestion": "Pivot to live workout sessions with friends rather than post-workout sharing",
        "rationale": "Real-time social interaction is an identified market gap with low competition"
      }
    ],
    "pivot_suggestions": [
      {
        "suggestion": "Target corporate wellness programs",
        "rationale": "Underserved B2B market with higher revenue potential"
      },
      {
        "suggestion": "Specialize in specific sports like weightlifting only",
        "rationale": "Niche focus allows for deeper expertise and less competition"
      }
    ],
    "likelihood_of_success": 72
  }
}
```

---

## 6. CHATBOT INTERACTION (FRONT-END AND BACK-END)

Users can ask questions about their validation results through the chatbot, which uses the analysis context to provide explanations and insights.

**Route**: `POST /api/chat`

**Chat Request**
```json
{
  "conversation_id": "conv_12345",
  "message": "Why is my likelihood of success only 72%?",
  "context": {
    "user_idea": {
      "title": "AI-Powered Fitness Tracker",
      "description": "A mobile app that uses *computer vision* to track workout form and provide *real-time feedback*. Features include real-time form analysis, personalized workout plans, progress tracking, and *social sharing capabilities*."
    },
    "analysis": {
      "competitor_analysis": {
        "direct_competitors": [
          {
            "name": "FormCheck AI",
            "strengths": ["Established user base", "Strong AI technology"],
            "weaknesses": ["Limited social features", "High price point"]
          }
        ]
      },
      "market_gaps": [
        "Real-time social interaction during workouts",
        "Integration with wearable devices"
      ],
      "likelihood_of_success": 72
    }
  }
}
```

**Chat Response**
```json
{
  "conversation_id": "conv_12345",
  "message": "Your 72% likelihood is based on having strong AI technology and addressing real user needs, but facing established competitors like FormCheck AI with existing user bases. The score factors in market saturation in fitness tracking (3 similar startups found) and the competitive landscape. However, you have opportunities in underserved areas like real-time social interaction, which could boost your success rate if you pivot focus there."
}
```
