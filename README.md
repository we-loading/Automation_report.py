# Automation_report.py
# File: automation_report.py
# Description: This program scans a directory, analyzes its files,
# generates a report and displays a summary.
# Assignment Number: 9
# Name: Yakubu Suhuyeni
# SID: 2425403511
# Email: kuburatuk@gmail.com
# Grader: Augustus Buckman
# On my honor, Yakubu Suhuyeni, this programming assignment is my own work
# and I have not provided this code to any other student.

import os
import datetime


def count_file_type(file_name, txt, py, csv, other):
    """Counts the different file types."""

    if file_name.endswith(".txt"):
        txt += 1
    elif file_name.endswith(".py"):
        py += 1
    elif file_name.endswith(".csv"):
        csv += 1
    else:
        other += 1

    return txt, py, csv, other


def update_oldest_newest(file_name, file_time,
                         oldest_file, newest_file,
                         oldest_time, newest_time):
    """Updates the oldest and newest files."""

    if oldest_time is None or file_time < oldest_time:
        oldest_time = file_time
        oldest_file = file_name

    if newest_time is None or file_time > newest_time:
        newest_time = file_time
        newest_file = file_name

    return (oldest_file, newest_file,
            oldest_time, newest_time)


def scan_directory(directory):
    """Scans the directory."""

    total = 0
    txt = 0
    py = 0
    csv = 0
    other = 0

    oldest_file = ""
    newest_file = ""
    oldest_time = None
    newest_time = None

    for file_name in os.listdir(directory):

        full_path = os.path.join(directory, file_name)

        if os.path.isfile(full_path):

            total += 1

            txt, py, csv, other = count_file_type(
                file_name, txt, py, csv, other)

            file_time = os.path.getmtime(full_path)

            (oldest_file, newest_file,
             oldest_time, newest_time) = update_oldest_newest(
                file_name, file_time,
                oldest_file, newest_file,
                oldest_time, newest_time)

    return (total, txt, py, csv,
            other, oldest_file, newest_file)


def write_report(directory, total, txt,
                 py, csv, other,
                 oldest, newest):
    """Writes the report to a file."""

    report = open("automation_report.txt", "w")

    report.write("AUTOMATION REPORT\n")
    report.write("=================\n")
    report.write("Directory: " + directory + "\n")
    report.write("Total Files: " + str(total) + "\n")
    report.write("Text Files: " + str(txt) + "\n")
    report.write("Python Files: " + str(py) + "\n")
    report.write("CSV Files: " + str(csv) + "\n")
    report.write("Other Files: " + str(other) + "\n")
    report.write("Oldest File: " + oldest + "\n")
    report.write("Newest File: " + newest + "\n")

    current = datetime.datetime.now()
    report.write("Generated: " + str(current) + "\n")

    report.close()


def display_summary(total, txt, py,
                    csv, other,
                    oldest, newest):
    """Displays the results."""

    print("Directory Analysis Complete")
    print("Total Files:", total)
    print("Text Files:", txt)
    print("Python Files:", py)
    print("CSV Files:", csv)
    print("Other Files:", other)
    print("Oldest File:", oldest)
    print("Newest File:", newest)
    print("Report written to automation_report.txt")


def main():
    """Main function."""

    print("Python Automation Report Generator")
    directory = input("Enter directory name: ")

    if not os.path.isdir(directory):
        print("Directory does not exist. Ending program.")
        return

    try:
        (total, txt, py, csv,
         other, oldest,
         newest) = scan_directory(directory)

        write_report(directory, total, txt,
                     py, csv, other,
                     oldest, newest)

        display_summary(total, txt, py,
                        csv, other,
                        oldest, newest)

    except PermissionError:
        print("Unable to access directory.")

    except Exception:
        print("An unexpected error occurred.")


main()
