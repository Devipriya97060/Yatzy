import random
import collections

DICE_COUNT = 5
ROLL_LIMIT = 3
CATEGORY_COUNT = 15
START = "start"
SKIP = "skip"

Player = collections.namedtuple("Player", ["name", "score_pad", "dice"])


def category_ones(dice):
    return dice.count(1)


def category_twos(dice):
    return dice.count(2) * 2


def category_threes(dice):
    return dice.count(3) * 3


def category_fours(dice):
    return dice.count(4) * 4


def category_fives(dice):
    return dice.count(5) * 5


def category_sixes(dice):
    return dice.count(6) * 6


def category_pair(dice):
    for i in range(6, 0, -1):
        if dice.count(i) >= 2:
            return i + i
    return 0


def category_two_pairs(dice):
    pair_count = 0
    score = 0
    for i in range(6, 0, -1):
        if dice.count(i) >= 2:
            score += (i + i)
            pair_count += 1
        if pair_count >= 2:
            break
    if pair_count >= 2:
        return score
    return 0


def category_three_of_a_kind(dice):
    for i in range(6, 0, -1):
        if dice.count(i) >= 3:
            return i * 3
    return 0


def category_four_of_a_kind(dice):
    for i in range(6, 0, -1):
        if dice.count(i) >= 4:
            return i * 4
    return 0


def category_small_straight(dice):
    found = True
    for i in range(1, 6):
        if i not in dice:
            found = False
            break
    if found:
        return 15
    return 0


def category_large_straight(dice):
    found = True
    for i in range(2, 7):
        if i not in dice:
            found = False
            break
    if found:
        return 20
    return 0


def category_full_house(dice):
    triple_found = False
    pair_found = False
    triple = 0
    pair = 0
    for i in range(6, 0, -1):
        count = dice.count(i)
        if count == 3:
            triple_found = True
            triple = i
        elif count == 2:
            pair_found = True
            pair = i

    if triple_found and pair_found:
        return 3 * triple + 2 * pair
    return 0


def category_chance(dice):
    return sum(dice)


def category_yatzy(dice):
    for i in range(1, 7):
        if dice.count(i) == 5:
            return 50
    return 0


def dice_gen(num):
    for x in range(num):
        yield random.randint(1, 6)


def roll_dice(dice):
    dice.clear()
    iteration = 1
    while iteration <= ROLL_LIMIT:
        if len(dice) == DICE_COUNT:
            break
        values = list(dice_gen(DICE_COUNT - len(dice)))
        print("Iteration - " + str(iteration))
        print(
            "Choose the dice you wish to keep by entering indexes as space seperated numbers or enter skip to roll all")

        print("[" + " ".join(list(map(str, values))) + "]")

        response = input()
        index_list = []
        if response.strip().lower() != SKIP:
            index_list = map(int, response.strip().split(" "))

        for i in index_list:
            dice.append(values[i - 1])

        iteration += 1
    return dice


def sort_by_score(item):
    return item['score']


def display_score(players):
    print("Displaying scores")
    scores = []
    for player in players:
        score_pad = player.score_pad
        total_score = 0
        for score in score_pad.values():
            total_score += score
        player_score = {'name': player.name, 'score': total_score}
        scores.append(player_score)

    scores.sort(reverse=True, key=sort_by_score)
    i = 0
    while i < len(scores):
        player = scores[i]
        print("{0} : {1}".format(player['name'], player['score']))
        i += 1


def run_game(categories_list, players):
    index = 1
    for category in categories_list:
        print("Playing Category - " + str(index) + " => " + category.__name__)
        for player in players:
            print(player + "'s turn :")
            dice = roll_dice(players[dice])
            score = category(players[dice])
            print("score : " + str(score))
            players[player].score_pad[category] = score
        index += 1
    display_score(players)


def main():
    categories_list = [category_ones, category_twos, category_threes, category_fours, category_fives, category_sixes,
                       category_pair, category_two_pairs, category_three_of_a_kind, category_four_of_a_kind,
                       category_small_straight, category_large_straight, category_full_house, category_chance,
                       category_yatzy]
    players = {}
    print("Enter player names followed by 'start' to start the game")
    while True:
        name = input()
        if name.strip().lower() == START:
            break
        players[name] = Player(name, {}, [0,0,0,0,0])
        #players.append(Player(name, {}, 5 * [0]))

    if len(players) == 0:
        return

    #random.shuffle(players)
    run_game(categories_list, players)


if __name__ == "__main__":
    main()
