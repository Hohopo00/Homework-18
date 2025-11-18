# Домашнее задание к работе 8


## Условие задачи
Написать структуру данных
## 1. Алгоритм 

### Алгоритм


## 2. Реализация программы

```
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
#include <locale.h>

#define MAX_PLAYERS 10

struct Date {
    int day;
    int month;
    int year;
};

struct Player {
    char surname[50];
    struct Date birth_date;
    char birth_city[50];
    char role[30];
    int games_played;
    int yellow_cards;
};


void print_center(const char* text);
int get_age(int birth_year);
int check_player(struct Player p);
void input_player(struct Player* p);
void print_player(struct Player p);


void print_center(const char* text) {
    printf("    %s\n", text);
}


int get_age(int birth_year) {
    return 2025 - birth_year;
}


int check_player(struct Player p) {
    int age = get_age(p.birth_date.year);

    
    if (age <= 20) {
        return 0;
    }

    
    if (p.yellow_cards <= 1) {
        return 1;
    }

    
    if (p.games_played > 0) {
        float cards_per_10_games = (float)p.yellow_cards / p.games_played * 10;
        if (cards_per_10_games <= 1.0) {
            return 1;
        }
    }

    return 0;
}


void input_player(struct Player* p) {
    printf("\nВвод данных игрока:\n");

    printf("Фамилия: ");
    scanf(" %[^\n]", p->surname);

    printf("Дата рождения (день месяц год): ");
    scanf("%d %d %d", &p->birth_date.day, &p->birth_date.month, &p->birth_date.year);

    printf("Город рождения: ");
    scanf(" %[^\n]", p->birth_city);

    printf("Амплуа: ");
    scanf(" %[^\n]", p->role);

    printf("Количество игр: ");
    scanf("%d", &p->games_played);

    printf("Количество желтых карточек: ");
    scanf("%d", &p->yellow_cards);

    
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}


void print_player(struct Player p) {
    printf("\n=== Данные игрока ===\n");
    printf("Фамилия: %s\n", p.surname);
    printf("Дата рождения: %02d.%02d.%d\n", p.birth_date.day, p.birth_date.month, p.birth_date.year);
    printf("Возраст: %d лет\n", get_age(p.birth_date.year));
    printf("Город рождения: %s\n", p.birth_city);
    printf("Амплуа: %s\n", p.role);
    printf("Игр: %d\n", p.games_played);
    printf("Желтых карточек: %d\n", p.yellow_cards);

    
    if (p.games_played > 0) {
        float cards_rate = (float)p.yellow_cards / p.games_played * 10;
        printf("Карточек за 10 игр: %.2f\n", cards_rate);
    }

    if (check_player(p)) {
        printf("Соответствует условию: ДА\n");
    }
    else {
        printf("Соответствует условию: НЕТ\n");
    }
}

int main() {
    setlocale(LC_ALL, "RUS");

    printf("=== Программа учета футболистов ===\n");
    printf("Условие: старше 20 лет и (0-1 карточка ИЛИ не более 1 карточки за 10 игр)\n");

    struct Player players[MAX_PLAYERS];
    int count = 0;
    int choice;

    do {
        printf("\n=== Меню ===\n");
        printf("1. Добавить игрока\n");
        printf("2. Показать всех игроков\n");
        printf("3. Показать игроков по условию\n");
        printf("4. Выход\n");
        printf("Выберите: ");
        scanf("%d", &choice);

        
        int c;
        while ((c = getchar()) != '\n' && c != EOF);

        switch (choice) {
        case 1:
            if (count < MAX_PLAYERS) {
                input_player(&players[count]);
                count++;
                printf("Игрок добавлен!\n");
            }
            else {
                printf("Достигнут максимум игроков!\n");
            }
            break;

        case 2:
            if (count == 0) {
                printf("Нет игроков!\n");
            }
            else {
                for (int i = 0; i < count; i++) {
                    print_player(players[i]);
                }
            }
            break;

        case 3:
            if (count == 0) {
                printf("Нет игроков!\n");
            }
            else {
                printf("\n=== Игроки по условию ===\n");
                printf("(старше 20 лет и 0-1 карточка или не более 1 карточки за 10 игр)\n");

                int found = 0;
                for (int i = 0; i < count; i++) {
                    if (check_player(players[i])) {
                        print_player(players[i]);
                        found++;
                    }
                }

                if (!found) {
                    printf("Не найдено игроков по условию.\n");
                }
                else {
                    printf("Найдено игроков: %d\n", found);
                }
            }
            break;

        case 4:
            printf("Выход из программы.\n");
            break;

        default:
            printf("Неверный выбор!\n");
        }

    } while (choice != 4);

    return 0;
}
```



## 4. Информация о разработчике

Прохорова Виктория, бТИИ-251
