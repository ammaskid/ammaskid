#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <unistd.h>

void increment_day(struct tm *time, int *day_in_month)
{
    int is_leap_year;
    int year = time->tm_year + 1900;
    if (year % 4 == 0)
    {
        if (year % 100 == 0)
        {
            // the year is a leap year if it is divisible by 400.
            if (year % 400 == 0)
                is_leap_year = 1;
            else
                is_leap_year = 0;
        }
        else
            is_leap_year = 1;
    }
    else
        is_leap_year = 0;
    day_in_month[1] = is_leap_year ? 29 : 28;

    int month = time->tm_mon;
    int total_days_in_month = day_in_month[month];

    if (time->tm_mday < total_days_in_month)
    {
        time->tm_mday++;
    }
    else
    {
        time->tm_mday = 1;
        if (month == 11)
        {
            time->tm_mon = 0;
        }
        else
        {
            time->tm_mon++;
        }
    }
}

int main(int argc, char **argv)
{
    int opt, year, month, day, count;

    while ((opt = getopt(argc, argv, ":y:m:d:c:h")) != -1)
    {
        switch (opt)
        {
        case 'y':
            year = atoi(optarg);
            printf("year: %d\n", year);
            break;
        case 'm':
            month = atoi(optarg);
            printf("month: %d\n", month);
            break;
        case 'd':
            day = atoi(optarg);
            printf("day: %d\n", day);
            break;
        case 'c':
            count = atoi(optarg);
            printf("count: %d\n", count);
            break;
        case 'h':
            printf("Enter numbers:\n-y: year\n-m: month\n-d: day\n-c count of days to paint\nEvery field must be entered\n");
            break;
        case ':':
            printf("option needs a value\n");
            break;
        case '?':
            printf("unknown option: %c\n", optopt);
            printf("Enter numbers:\n-y: year\n-m: month\n-d: day\n-c count of days to paint\nEvery field must be entered\n");
            break;
        }
    }
    for (; optind < argc; optind++)
    {
        printf("extra arguments: %s\n", argv[optind]);
    }

    if (!(year && month && day && count))
    {
        perror("year, month, day, count arguments must be entered and non-zero\n");
        exit(1);
    }

    int day_in_month[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    struct tm t = {.tm_year = year - 1900, .tm_mon = month - 1, .tm_mday = day, .tm_hour = 10};
    time_t set_time, now_time;
    set_time = mktime(&t);
    time(&now_time);
    int i = 0;
    char buffer[50];
    while (now_time - set_time > 0 && i < count)
    {
        char *time_string = asctime(&t);
        sprintf(buffer, "%d-%d-%dT%d:%d:%d", t.tm_year + 1900, t.tm_mon + 1, t.tm_mday, t.tm_hour, t.tm_min, t.tm_sec);
        setenv("MODIFIED_TIME", buffer, 1);
        system("sh modify_time.sh");
        increment_day(&t, day_in_month);
        i++;
    }
    system("git push");
    return 0;
}