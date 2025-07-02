
# LaLiga Performance Analysis Report Description
Dataset used https://www.kaggle.com/datasets/marcelbiezunski/laliga-matches-dataset-2019-2025-fbref

## Project Overview

This comprehensive football analytics report examines the performance patterns of Spanish LaLiga teams across six seasons (2020-2025), providing detailed insights into team effectiveness, goal-scoring trends, and competitive dynamics within Spain's premier football league.

## Key Research Questions Addressed

### 1. **Team Performance Consistency**
- Which teams maintain the highest win rates across multiple seasons?
- How do elite teams (Real Madrid, Barcelona, Atletico Madrid) compare in terms of consistent performance?
- What are the performance fluctuations of mid-tier and lower-tier teams?

### 2. **Goal-Scoring Evolution**
- How has the average goals per match evolved across the six-season period?
- Which teams demonstrate the most effective attacking strategies?
- What is the relationship between expected goals (xG) and actual goal performance?

### 3. **Match Outcome Distribution**
- What is the overall distribution of wins, draws, and losses across all teams?
- How do seasonal variations affect match outcomes?
- Which teams show improvement or decline in recent seasons?

## Analytical Processes and Methodology

### **Data Collection and Preparation**
- Comprehensive dataset covering 27 LaLiga teams across 6 seasons
- Match-level data including goals scored, match outcomes, and expected goals (xG)
- Seasonal aggregation and team-level performance metrics

### **Performance Metrics Calculation**
- **Win Rate Analysis**: Percentage of matches won per team per season
- **Goal Efficiency**: Average goals scored per match by season
- **Expected Goals Comparison**: Actual vs predicted goal performance
- **Outcome Distribution**: Classification of all matches into wins, draws, and losses

### **Visualization and Dashboard Creation**
- Interactive Power BI dashboard with multiple analytical views
- Team filtering capabilities for focused analysis
- Seasonal trend visualization with temporal comparisons
- Performance ranking systems for competitive benchmarking

## Key Project Insights

### **Elite Team Dominance**
- **Real Madrid** emerges as the most consistent performer with a 68% overall win rate
- **Barcelona** maintains strong performance (66% win rate) despite some seasonal fluctuations
- **Atletico Madrid** shows significant improvement trajectory, reaching 59% overall win rate

### **Competitive Landscape Evolution**
- **Girona** demonstrates remarkable emergence as a competitive force in recent seasons
- Traditional mid-tier teams like **Sevilla** show declining performance trends
- **Real Sociedad** maintains consistent mid-table performance around 45% win rate

### **Goal-Scoring Trends**
- Slight upward trend in average goals per match from 2020 to 2025
- Peak goal-scoring periods observed in 2023-2024 seasons
- Strong correlation between expected goals (xG) and actual performance for top teams

### **Match Outcome Patterns**
- Overall league distribution: 36% wins, 36% draws, 27% losses per team
- Increasing competitiveness evidenced by fluctuating draw rates
- Seasonal variations suggest evolving tactical approaches across the league

## Final Conclusions

### **Performance Hierarchy Stability**
The analysis reveals a relatively stable competitive hierarchy in LaLiga, with Real Madrid and Barcelona maintaining their positions as dominant forces. However, the emergence of teams like Girona and the decline of traditionally strong teams like Sevilla indicate that the league's competitive balance is evolving.

### **Tactical Evolution**
The gradual increase in average goals per match suggests an evolution toward more offensive-minded football across the league, possibly reflecting tactical innovations and rule changes that favor attacking play.

### **Predictive Accuracy**
The strong alignment between expected goals (xG) and actual performance for top teams validates the effectiveness of modern football analytics in predicting team success, while also highlighting which teams overperform or underperform relative to their underlying metrics.

### **Strategic Implications**
For football analysts, coaches, and management:
- **Performance sustainability** requires consistent tactical adaptation
- **Expected goals metrics** serve as reliable indicators of long-term team quality
- **Seasonal variations** emphasize the importance of squad depth and tactical flexibility
- **Emerging teams** like Girona demonstrate that strategic planning can rapidly elevate competitive position

This analysis provides a foundational understanding of LaLiga's competitive dynamics and serves as a valuable resource for strategic decision-making in football management, player recruitment, and tactical development.

# LaLiga Analysis Project - Data Processing Steps

## 1. Data Collection Phase

### **Data Sources**
- **Primary Source**: LaLiga match data covering 6 seasons (2020-2025)
- **Data Scope**: 27 teams across multiple seasons
- **Key Data Points**: Match results, goals scored, expected goals (xG), team formations, seasonal performance

### **Data Acquisition Methods**
- Web scraping from kaggle


## 2. Data Import and Initial Setup

### **Power BI Data Import**

Power BI Desktop → Get Data → Connect to Data Source
- File formats: CSV, Excel, JSON, or API connections
- Table created: 'La Liga Analysis'
- Initial data validation and preview


### **Data Source Verification**
- Confirmed data completeness across all 6 seasons
- Verified team names consistency (27 teams total)
- Checked date ranges and seasonal boundaries
- Validated numerical data types (goals, xG values)

## 3. Data Cleaning and Transformation

### **Data Quality Assessment**
- **Missing Values**: Identified and handled null/blank entries
- **Duplicate Records**: Removed duplicate match entries
- **Data Types**: Converted text to proper formats (dates, numbers)
- **Standardization**: Unified team names across different seasons

### **Power Query Transformations**
M
// Example Power Query steps:
= Table.TransformColumnTypes(Source,{
    {"Date", type date}, 
    {"Goals_Scored", Int64.Type},
    {"Expected_Goals(xG)", type number}, 
    {"Season", type text},
    {"Team_Name", type text}
})

### **Data Standardization**
- **Team Names**: Ensured consistent naming (e.g., "Atletico Madrid" vs "Atlético Madrid")
- **Season Format**: Standardized season representation (2020, 2021, etc.)
- **Match Results**: Categorized outcomes (Win, Draw, Loss)
- **Formation Data**: Cleaned formation strings (4-3-3, 4-4-2, etc.)

## 4. Data Modeling and Relationships

### **Table Structure Design**

La Liga Analysis (Fact Table)
├── Match_ID (Primary Key)
├── Date
├── Season
├── Team_Name
├── Opponent
├── Goals_Scored
├── Goals_Conceded
├── Expected_Goals(xG)
├── Result (Win/Draw/Loss)
├── Formation
└── Additional match metrics


### **Calculated Columns Creation**
DAX
// Win Rate Calculation
Win_Rate = 
DIVIDE(
    COUNTROWS(FILTER('La Liga Analysis', 'La Liga Analysis'[Result] = "Win")),
    COUNTROWS('La Liga Analysis')
)

// Goals Difference
Goal_Difference = 'La Liga Analysis'[Goals_Scored] - 'La Liga Analysis'[Goals_Conceded]

// Points Calculation
Points = 
SWITCH(
    'La Liga Analysis'[Result],
    "Win", 3,
    "Draw", 1,
    "Loss", 0
)


## 5. Data Validation and Quality Control

### **Statistical Validation**
- **Range Checks**: Verified goals scored within realistic ranges (0-10)
- **Consistency Checks**: Match results align with goal differences
- **Seasonal Totals**: Validated total matches per season per team
- **Expected Goals**: Confirmed xG values are logical compared to actual goals

### **Business Logic Validation**
- **Team Participation**: All 27 teams have data across relevant seasons
- **Match Balance**: Each match appears for both participating teams
- **Seasonal Structure**: Proper match distribution across seasons
- **Formation Logic**: Formation data follows standard football formations

## 6. Advanced Analytics Implementation

### **Performance Metrics Development**
DAX
// Average Goals Per Match by Season
Avg_Goals_Season = 
AVERAGEX(
    SUMMARIZE('La Liga Analysis', 'La Liga Analysis'[Season]),
    CALCULATE(AVERAGE('La Liga Analysis'[Goals_Scored]))
)

// Team Win Rate Over Time
Team_Win_Rate_Trend = 
CALCULATE(
    DIVIDE(
        COUNTROWS(FILTER('La Liga Analysis', 'La Liga Analysis'[Result] = "Win")),
        COUNTROWS('La Liga Analysis')
    )
)


### **Formation Analysis Setup**
- Created formation frequency measures
- Developed formation success rate calculations
- Built team-specific formation preference metrics
- Implemented seasonal formation trend analysis

## 7. Dashboard Development Process

### **Visual Design Planning**
1. **KPI Cards**: Total wins, draws, losses, average goals
2. **Trend Charts**: Goals per match over seasons
3. **Comparison Charts**: Team win rates, xG vs actual goals
4. **Interactive Filters**: Team selection, season filters
5. **Distribution Charts**: Match results breakdown

### **Power BI Visualization Steps**
```
1. Import cleaned data → La Liga Analysis table
2. Create measures for key metrics
3. Build individual visualizations
4. Apply consistent formatting and themes
5. Add interactive filters and slicers
6. Test dashboard functionality
7. Optimize performance and user experience
```

## 8. Data Refresh and Maintenance

### **Automated Data Updates**
- **Scheduled Refresh**: Set up automatic data refresh in Power BI Service
- **Version Control**: Maintained backup copies of clean datasets
- **Error Handling**: Implemented data validation checks for new data

### **Data Quality Monitoring**
- **Regular Audits**: Monthly checks for data completeness
- **Anomaly Detection**: Automated alerts for unusual values
- **Performance Monitoring**: Dashboard load times and query performance
- **User Feedback Integration**: Incorporated user suggestions for improvements

## 9. Testing and Validation Phase

### **Dashboard Testing**
- **Functionality Testing**: All filters and interactions work correctly
- **Data Accuracy**: Spot-checked calculations against source data
- **Performance Testing**: Optimized for reasonable load times
- **Cross-Validation**: Verified metrics against external football statistics

### **User Acceptance Testing**
- **Stakeholder Review**: Presented to football analysts/fans
- **Feedback Incorporation**: Made adjustments based on user needs
- **Documentation**: Created user guides for dashboard navigation
- **Training**: Provided guidance on interpreting the analytics

## 10. Deployment and Documentation



### **Project Documentation**
- **Technical Documentation**: Detailed data model and measure explanations
- **User Manual**: Guide for end-users on dashboard navigation
- **Data Dictionary**: Definitions of all fields and calculations
- **Maintenance Guide**: Instructions for future updates and modifications

## Tools and Technologies Used

### **Primary Tools**
- **Power BI Desktop**: Main development environment
- **Power Query**: Data transformation and cleaning
- **DAX**: Advanced calculations and measures
- **Power BI Service**: Cloud deployment and sharing


This comprehensive data processing approach ensured a robust, accurate, and user-friendly LaLiga analysis dashboard that provides valuable insights into Spanish football performance trends.
