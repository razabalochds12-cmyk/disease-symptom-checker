# ==================================================
# AI EXPERT SYSTEM: DISEASE SYMPTOM CHECKER ==================================================

def line():
    print("=" * 48)


def show_header():
    line()
    print("       AI MEDICAL EXPERT SYSTEM")
    print("        DISEASE SYMPTOM CHECKER")
    line()
    print("  Educational Rule-Based AI Project")
    line()


SYMPTOMS = [
    "Fever",
    "Cough",
    "Sore throat",
    "Headache",
    "Body pain",
    "Runny nose",
    "Sneezing",
    "Nausea",
    "Vomiting",
    "Diarrhea",
    "Stomach pain",
    "Fatigue",
    "Shortness of breath",
    "Chest pain",
    "Loss of smell"
]


# Simple IF-THEN knowledge base
RULES = [
    {
        "disease": "Common Cold",
        "symptoms": {
            "Runny nose", "Sneezing", "Cough",
            "Sore throat"
        },
        "advice": "Rest and drink fluids."
    },
    {
        "disease": "Flu-like Illness",
        "symptoms": {
            "Fever", "Cough", "Body pain",
            "Headache", "Fatigue"
        },
        "advice": "Rest and seek medical advice if symptoms are severe."
    },
    {
        "disease": "Possible Gastroenteritis",
        "symptoms": {
            "Nausea", "Vomiting", "Diarrhea",
            "Stomach pain"
        },
        "advice": "Drink fluids. Seek care for dehydration or severe pain."
    },
    {
        "disease": "Possible Respiratory Infection",
        "symptoms": {
            "Fever", "Cough", "Sore throat",
            "Fatigue"
        },
        "advice": "Consult a healthcare professional if symptoms persist."
    },
    {
        "disease": "Possible Migraine Pattern",
        "symptoms": {
            "Headache", "Nausea", "Vomiting"
        },
        "advice": "Rest in a quiet room and seek medical advice."
    }
]


def get_patient():
    print("\nPATIENT INFORMATION")
    name = input("Enter patient name: ").strip()
    if not name:
        name = "Not provided"

    while True:
        try:
            age = int(input("Enter patient age: "))
            if 0 <= age <= 120:
                break
            print("Please enter an age from 0 to 120.")
        except ValueError:
            print("Enter age as a whole number.")

    return name, age


def select_symptoms():
    print("\nAVAILABLE SYMPTOMS")
    for i, symptom in enumerate(SYMPTOMS, 1):
        print(f"{i:2}. {symptom}")

    print("\nEnter symptom numbers separated by commas.")
    print("Example: 1, 2, 4")

    while True:
        raw = input("Your selection: ")
        try:
            numbers = [
                int(x.strip())
                for x in raw.split(",")
                if x.strip()
            ]

            if len(numbers) != len(set(numbers)):
                print("Repeated numbers found. Try again.")
                continue

            if any(n < 1 or n > len(SYMPTOMS)
                   for n in numbers):
                print("Choose numbers from the list.")
                continue

            if len(numbers) < 3:
                print("Please select at least 3 symptoms.")
                continue

            return {SYMPTOMS[n - 1] for n in numbers}

        except ValueError:
            print("Invalid input. Example: 1, 3, 5")


def analyze(selected):
    results = []

    for rule in RULES:
        matched = selected.intersection(rule["symptoms"])
        if len(matched) >= 2:
            results.append({
                "disease": rule["disease"],
                "matched": matched,
                "score": len(matched),
                "advice": rule["advice"]
            })

    results.sort(key=lambda item: item["score"],
                 reverse=True)
    return results


def show_results(name, age, selected):
    line()
    print("          EXPERT SYSTEM REPORT")
    line()
    print("Patient:", name)
    print("Age:", age)
    print("\nSelected symptoms:")
    for symptom in sorted(selected):
        print(" -", symptom)

    print("\nRULE-BASED ANALYSIS")
    line()

    # Safety alert takes priority over matching rules.
    if "Chest pain" in selected or \
            "Shortness of breath" in selected:
        print("URGENT SAFETY ALERT")
        print("Chest pain or breathing difficulty can")
        print("indicate a serious problem.")
        print("Seek urgent medical assessment now.")
        print("Call local emergency services for severe")
        print("or sudden symptoms.")
        line()
        return

    results = analyze(selected)

    if not results:
        print("No matching rule found.")
        print("This does not rule out an illness.")
    else:
        for i, result in enumerate(results[:3], 1):
            print(f"\n{i}. {result['disease']}")
            print("Matching symptoms:",
                  ", ".join(sorted(result["matched"])))
            print("Rule matches:",
                  result["score"])
            print("General advice:", result["advice"])

    line()
    print("IMPORTANT MEDICAL DISCLAIMER")
    print("This is a student demonstration, not a")
    print("clinically validated diagnostic system.")
    print("It cannot confirm or exclude any disease.")
    print("Consult a qualified healthcare professional.")
    line()


def main():
    while True:
        show_header()
        name, age = get_patient()
        selected = select_symptoms()
        show_results(name, age, selected)

        again = input(
            "\nCheck another patient? (y/n): "
        ).strip().lower()

        if again != "y":
            print("\nThank you for using the Expert System!")
            break


if __name__ == "__main__":
    main()
