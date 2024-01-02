## Objective


## Solution

```python3
# Error in function named play_cards:

def play_cards(csv_card_choices, request_id):
    try:
        f = StringIO(csv_card_choices)
        reader = csv.reader(f, delimiter=',')
        player_cards = []

        for row in reader:
            for n in row:
                n = float(n)
                if is_valid_whole_number_choice(n) and n not in [x['num'] for x in player_cards]:
                    player_cards.append({
                        'owner': 'p',
                        'num': n
                    })
            break

        if len(player_cards) != 5:
            return jsonify({
                "request": False,
                "data": f"Requires 5 unique values but was given \"{csv_card_choices}\""
            })

        player_cards = sorted(player_cards, key=lambda d: d['num'])
        shiftys_cards = shifty_mcshuffles_choices(player_cards)
        all_cards = []

        for p in player_cards:
            if p['num'] not in [x['num'] for x in shiftys_cards]:
                all_cards.append(p)

        for s in shiftys_cards:
            if s['num'] not in [x['num'] for x in player_cards]:
                all_cards.append(s)

        maxItem = False
        minItem = False

        if bool(len(all_cards)):
            maxItem = max(all_cards, key=lambda x: x['num'])
            minItem = min(all_cards, key=lambda x: x['num'])

        p_starting_value = int(session.get('player', 0))
        s_starting_value = int(session.get('shifty', 0))

        if bool(maxItem):
            if maxItem['owner'] == 'p':
                session['player'] = str(p_starting_value + 1)
            else:
                session['shifty'] = str(s_starting_value + 1)

        if bool(minItem):
            if minItem['owner'] == 'p':
                session['player'] = str(int(session.get('player', 0)) + 1)
            else:
                session['shifty'] = str(int(session.get('shifty', 0)) + 1)

        score_message, win_lose_tie_na = win_lose_tie_na_calc(
            int(session.get('player', 0)), int(session.get('shifty', 0))
        )

        play_message = 'Ha, we tied!'

        if int(session['player']) - p_starting_value > int(session['shifty']) - s_starting_value:
            play_message = 'Darn, how did I lose that hand!'
        elif int(session['player']) - p_starting_value < int(session['shifty']) - s_starting_value:
            play_message = 'I win and you lose that hand!'

        if win_lose_tie_na in ['w', 'l', 't']:
            session['player'] = '0'
            session['shifty'] = '0'

        msg = {
            "request": True,
            "data": {
                'player_cards': player_cards,
                'shiftys_cards': shiftys_cards,
                'maxItem': maxItem,
                'minItem': minItem,
                'player_score': int(session['player']),
                'shifty_score': int(session['shifty']),
                'score_message': score_message,
                'win_lose_tie_na': win_lose_tie_na,
                'play_message': play_message,
            }
        }

        if win_lose_tie_na == "w":
            msg["data"]['conduit'] = {
                'hash': hmac.new(submissionKey.encode('utf8'), request_id.encode('utf8'), sha256).hexdigest(),
                'resourceId': request_id
            }

        return jsonify(msg)

    except Exception as e:
        err = f"{type(e).__name__} at line {e.__traceback__.tb_lineno} of {__file__}: {e}"
        raise ValueError(err)

```
