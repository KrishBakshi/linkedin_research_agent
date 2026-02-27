---
name: linkedin-boolean-search
description: Constructs advanced LinkedIn Boolean search queries using AND, OR operators, and quotation marks with LinkedIn-specific filters. Use when the user wants to search LinkedIn, find candidates, create recruiter searches, build talent sourcing queries, or needs help with LinkedIn search operators.
---

# LinkedIn Boolean Search Query Builder

## Core Boolean Operators

LinkedIn supports these primary Boolean operators:

- **AND**: All terms must be present (implied by default, can be explicit)
- **OR**: At least one term must be present
- **Quotation marks `""`**: Exact phrase matching

## Basic Syntax Rules

1. **Case insensitive**: Operators work in any case, but UPPERCASE is recommended for clarity
2. **Implicit AND**: Space between terms defaults to AND
3. **Quotes**: Use for exact job titles, company names, or multi-word phrases and location

## Filter rules
1. Apply filters for locations, company, and connection type only
2. Use filters when finding specific locations, etc. is explicitly mentioned
3. Navigate using 'linkedin_search_page_navigation' skill

## Search Pattern Examples

```
developer OR engineer AND python OR java
```
Multiple role variations with required skills.

```
"product manager" AND "london"
```
Exact phrase with location keyword within quotes.

```
react OR angular AND typescript
```
Skills combination with alternatives.

## LinkedIn-Specific Search Filters

While Boolean operators work in the main search, combine them with LinkedIn's built-in filters for precision:

### Available Filters (apply after search if specified, refer 'linkedin_search_page_naviagtion' skill for navigation)
- **1st, 2nd, 3rd+ connections**: Filter by network proximity
- **Location**: Specific cities, regions, or countries
- **Current companies**: Filter by current employer
- **Past companies**: Find people who worked at specific companies
- **Industries**: Target specific industry sectors
- **Profile language**: Search profiles in specific languages
- **Schools**: Find alumni from specific universities
- **Service categories**: For service providers

### Location Filter Usage Rules
- **"Based in [Location]"**: Apply the specific Location filter for that region/country.
  - *Example*: "Find AI engineers based in Japan" -> Apply `Location: Japan` filter.
- **"Associated with [Location]"**: Do NOT apply the Location filter. Instead, include the location name as a keyword in the Boolean string.
  - *Example*: "Find people associated with Japan" -> Add `AND "Japan"` to the query string, but do not restrict the Location filter (to catch expats, people with past education/work there, etc.).

## Advanced Query Examples

```
"software engineer" OR "senior developer" AND aws OR azure AND python
```
Complex criteria: senior roles with cloud and Python skills.

```
"ai engineer" OR "ml engineer" AND pytorch
```
AI/ML roles with specific framework experience.

## Critical Rules

1. **Max 2-4 OR terms per query** - LinkedIn has complexity limits
   - Bad: `"ai" OR "ml" OR "data science" OR "analytics"` 
   - Good: `"ai" OR "data science"`

2. **Never use short AND long forms of same term**
   - Bad: `"ml" OR "machine learning"` or `"ai" OR "artificial intelligence"`
   - Good: `"ai" OR "ml"` - different concepts, both short
   - Always use short forms: "ai", "ml" not "artificial intelligence", "machine learning"

3. **Use quotes for multi-word phrases**
   - Bad: `product manager`
   - Good: `"product manager"`

## Query Examples by Role

**Engineers:**
```
"senior software engineer" OR "staff engineer" AND "distributed systems" OR microservices
```

**Data Professionals:**
```
"data analyst" OR "data scientist" AND sql AND python
```

**AI/ML Engineers:**
```
"ai engineer" OR "ml engineer" AND pytorch OR tensorflow
```

**AI/ML Founders:**
```
"founder" OR "co-founder" AND "ai" OR "data science" AND japan
```

## Best Practices

1. **Use short forms**: "ai", "ml" not "artificial intelligence", "machine learning"
2. **Keep compact**: 2-4 OR terms per query maximum
3. **Start simple**: Core role + 1-2 skills, then add complexity
4. **Use quotes**: For exact phrases like `"product manager"`
5. **Operator precedence**: AND > OR

## Operator Usage

- **AND**: Required terms (skills, experience)
- **OR**: Alternatives (role variations, equivalent skills)
- **Quotes**: Exact phrases (job titles, certifications)
