EX.NO.09 – BUILDING A SIMPLE AI AGENT: AI TOURIST GUIDE FOR INDIA
Aim

To design, implement, and test a simple goal-based AI agent in Python that creates a personalised India trip itinerary based on the tourist's interest, trip duration, and daily budget.

Introduction

An AI agent is a system that perceives its environment through sensors and acts upon the environment through actuators to achieve a specific goal.

The PEAS framework is commonly used to describe an intelligent agent:

P – Performance Measure: Determines how successfully the agent achieves its goal.
E – Environment: The surroundings or information in which the agent operates.
A – Actuators: Actions performed by the agent.
S – Sensors: Information perceived by the agent.

AI agents can be classified into:

Simple Reflex Agents: React only to the current input.
Goal-Based Agents: Select actions that help achieve a defined goal.
Utility-Based Agents: Select actions that maximise a measure of overall usefulness or quality.

In this experiment, an AI Tourist Guide for India is developed as a goal-based agent. It receives a tourist's interests, number of travel days, and daily budget, searches its destination knowledge base, plans an itinerary, and presents the recommended trip.

Procedure
Step 1: Import Required Libraries

The textwrap library is imported to neatly format and wrap long destination descriptions while displaying the itinerary.

import textwrap

Step 2: Define the Agent's Knowledge Base

The agent's knowledge about Indian tourist destinations is stored as a list of dictionaries.

Each destination contains:

Category: Heritage, spiritual, beach, hill station, adventure, nature, or wildlife.
Estimated cost per day
Best travel season
Description

Example:

destinations = [
    {
        "name": "Taj Mahal",
        "category": "heritage",
        "cost": 2500,
        "season": "October to March",
        "description": "A world-famous monument in Agra."
    },
    {
        "name": "Goa",
        "category": "beach",
        "cost": 4000,
        "season": "November to February",
        "description": "A popular destination known for beaches and coastal activities."
    }
]


This knowledge base acts as the environment information over which the goal-based agent reasons.

Step 3: Perceive the Tourist's Goal

The perceive() function represents the agent's sensors.

It reads the tourist's:

Interest/category
Number of days
Daily budget

The agent then displays the received information to confirm the tourist's requirements.

def perceive(tourist):
    print("Tourist Profile")
    print("Interest:", tourist["interest"])
    print("Days:", tourist["days"])
    print("Daily Budget: Rs.", tourist["budget"])

Step 4: Reason – Filter and Rank Destinations

The agent compares the tourist's requirements with every destination in its knowledge base.

A destination is selected when:

Its category matches the tourist's interest.
Its estimated daily cost is within the tourist's budget.

If no destination satisfies the exact budget, the agent selects destinations from the requested category and sorts them from cheapest to most expensive.

def find_destinations(interest, budget):
    matches = [
        d for d in destinations
        if d["category"] == interest and d["cost"] <= budget
    ]

    if not matches:
        matches = [
            d for d in destinations
            if d["category"] == interest
        ]
        matches.sort(key=lambda x: x["cost"])

    return matches

Step 5: Plan – Build a Day-wise Itinerary

The agent creates an itinerary based on the selected destinations.

A maximum of 3 days is allocated to one destination.
After 3 days, the agent moves to the next suitable destination.
For longer trips, it cycles through the matching destinations again.
Consecutive days at the same destination are combined into one itinerary block.
The estimated total cost is calculated automatically.
def plan_trip(matches, days):
    itinerary = []
    total_cost = 0
    day = 1
    index = 0

    while day <= days:
        destination = matches[index % len(matches)]
        stay = min(3, days - day + 1)

        itinerary.append({
            "destination": destination,
            "start_day": day,
            "end_day": day + stay - 1
        })

        total_cost += destination["cost"] * stay
        day += stay
        index += 1

    return itinerary, total_cost

Step 6: Act – Present the Recommendation

The act() function represents the agent's actuator.

It displays:

Destination name
Day range
Description
Best travel season
Estimated daily cost
Total estimated trip cost
def act(itinerary, total_cost):
    print("\nRecommended Itinerary")

    for item in itinerary:
        d = item["destination"]

        print(
            f"Day {item['start_day']}-{item['end_day']}: "
            f"{d['name']}"
        )

        print("Season:", d["season"])
        print("Daily Cost: Rs.", d["cost"])

        print(textwrap.fill(
            d["description"],
            width=70
        ))

    print("\nTotal Estimated Cost: Rs.", total_cost)

Step 7: Agent Loop

The complete agent follows the cycle:

Perceive → Reason → Plan → Act

The run_agent() function connects all four stages.

def run_agent(tourist):
    perceive(tourist)

    matches = find_destinations(
        tourist["interest"],
        tourist["budget"]
    )

    itinerary, total_cost = plan_trip(
        matches,
        tourist["days"]
    )

    act(itinerary, total_cost)


This cycle represents the basic operation of a goal-based AI agent.

Step 8: Test the Agent

Three tourist profiles can be used to test the system:

Budget Heritage Traveller

Interest: Heritage
Duration: 6 days
Limited daily budget

Adventure Seeker

Interest: Adventure
Duration: Several days
Budget: ₹5,000 per day

Beach Holiday Family

Interest: Beach
Duration: 7 days
Family travel budget
Output
Session 1 – Budget Heritage Traveller

The agent perceives the tourist's requirements and identifies suitable heritage destinations such as Taj Mahal and Jaipur that satisfy the budget.

It then creates a 6-day itinerary, distributing the days between the matching destinations and calculating the estimated total cost.

Sessions 2 and 3 – Adventure and Beach Holidays

For the adventure traveller, the agent selects destinations such as Coorg and Spiti Valley that fit the ₹5,000/day budget.

For the beach-holiday family, the agent selects destinations such as Goa and the Andaman Islands and distributes them across the requested 7 days.

The total estimated cost is calculated automatically for each itinerary.

Result

The simple goal-based AI Tourist Guide successfully perceives the tourist's preferences, identifies suitable destinations, plans a day-wise itinerary, and calculates the estimated trip cost.

Conclusion

Thus, a simple goal-based AI Tourist Agent for India was successfully designed, implemented, and tested using Python.

The agent follows the Perceive → Reason → Plan → Act cycle. It perceives the tourist's interest, duration, and budget, reasons over a destination knowledge base, plans a personalised itinerary, and acts by presenting a complete recommendation with estimated costs.

This experiment demonstrates the fundamental components of autonomous AI agents—environment knowledge, perception, reasoning, planning, and action—which can be extended using machine learning, real-time APIs, and large language models.
