#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <time.h>
#include <string>
#include <windows.h>

using namespace std;
int deck, card, deckk, c, player;
string cardh[416][4];

unsigned int times;

void cardbase() {
    for (int i = 0; i < deck; i++) {                                                      //카드집의 갯수.
        for (int j = 0; j < 13; j++) {                                                 //무늬.
            cardh[i * 52 + j][0] = "spd";
            cardh[i * 52 + 13 + j][0] = "dim";
            cardh[i * 52 + 26 + j][0] = "clv";
            cardh[i * 52 + 39 + j][0] = "hrt";
        }

        for (int j = 0; j < 4; j++) {                                                  //숫자 - 무늬
            for (int k = 0; k < 13; k++) {                                              //숫자
                if (k == 0) { cardh[i * 52 + j * 13 + k][1] = "A"; }
                else if (k == 1) { cardh[i * 52 + j * 13 + k][1] = "2"; }
                else if (k == 2) { cardh[i * 52 + j * 13 + k][1] = "3"; }
                else if (k == 3) { cardh[i * 52 + j * 13 + k][1] = "4"; }
                else if (k == 4) { cardh[i * 52 + j * 13 + k][1] = "5"; }
                else if (k == 5) { cardh[i * 52 + j * 13 + k][1] = "6"; }
                else if (k == 6) { cardh[i * 52 + j * 13 + k][1] = "7"; }
                else if (k == 7) { cardh[i * 52 + j * 13 + k][1] = "8"; }
                else if (k == 8) { cardh[i * 52 + j * 13 + k][1] = "9"; }
                else if (k == 9) { cardh[i * 52 + j * 13 + k][1] = "T"; }
                else if (k == 10) { cardh[i * 52 + j * 13 + k][1] = "J"; }
                else if (k == 11) { cardh[i * 52 + j * 13 + k][1] = "Q"; }
                else if (k == 12) { cardh[i * 52 + j * 13 + k][1] = "K"; }
            }
        }

        for (int j = 0; j < 52; j++) {                                                    //댁번호
            if (i == 0) { cardh[j][2] = "fst"; }
            else if (i == 1) { cardh[i * 52 + j][2] = "scd"; }
            else if (i == 2) { cardh[i * 52 + j][2] = "thd"; }
            else if (i == 3) { cardh[i * 52 + j][2] = "frt"; }
            else if (i == 4) { cardh[i * 52 + j][2] = "fth"; }
            else if (i == 5) { cardh[i * 52 + j][2] = "sxt"; }
            else if (i == 6) { cardh[i * 52 + j][2] = "svt"; }
            else if (i == 7) { cardh[i * 52 + j][2] = "eth"; }
        }
    }
}

void hip() {
    for (int i = 1; i < deckk; i++) {                                                        //힙정렬
        int c = i;
        do {
            int root = (c - 1) / 2;
            if (stoull(cardh[root][2]) < stoull(cardh[c][2])) {
                string temp2 = cardh[root][2];
                cardh[root][2] = cardh[c][2];
                cardh[c][2] = temp2;

                string temp1 = cardh[root][1];
                cardh[root][1] = cardh[c][1];
                cardh[c][1] = temp1;

                string temp0 = cardh[root][0];
                cardh[root][0] = cardh[c][0];
                cardh[c][0] = temp0;
            }
            c = root;
        } while (c != 0);
    }
    for (int i = deckk - 1; i >= 0; i--) {
        string temp2 = cardh[0][2];
        cardh[0][2] = cardh[i][2];
        cardh[i][2] = temp2;

        string temp1 = cardh[0][1];
        cardh[0][1] = cardh[i][1];
        cardh[i][1] = temp1;

        string temp0 = cardh[0][0];
        cardh[0][0] = cardh[i][0];
        cardh[i][0] = temp0;

        int root = 0;
        int c = 1;
        do {
            c = 2 * root + 1;
            if (c < i - 1 && stoull(cardh[c][2]) < stoull(cardh[c + 1][2])) {
                c++;
            }
            if (c < i && stoull(cardh[root][2]) < stoull(cardh[c][2])) {
                temp2 = cardh[root][2];
                cardh[root][2] = cardh[c][2];
                cardh[c][2] = temp2;

                temp1 = cardh[root][1];
                cardh[root][1] = cardh[c][1];
                cardh[c][1] = temp1;

                temp0 = cardh[root][0];
                cardh[root][0] = cardh[c][0];
                cardh[c][0] = temp0;
            }
            root = c;
        } while (c < i);
    }
}

class Well512Random {                                                                     //well512

protected:
    unsigned int state[16];
    unsigned int index;

public:
    Well512Random(unsigned int nSeed) {
        index = 0;
        unsigned int s = nSeed;
        for (int i = 0; i < 16; i++) {
            state[i] = s;
            s += s + 73;
        }
    }
    unsigned int Next(int minValue, int maxValue) {
        return (unsigned int)((Next() % (maxValue - minValue)) + minValue);
    }

    unsigned int Next(unsigned int maxValue) {
        return Next() % maxValue;
    }

    unsigned int Next() {
        unsigned int a, b, c, d;
        a = state[index];
        c = state[(index + 13) & 15];
        b = a ^ c ^ (a << 16) ^ (c << 15);
        c = state[(index + 9) & 15];
        c ^= (c >> 11);
        a = state[index] = b ^ c;
        d = a ^ ((a << 5) & 0xda442d24U);
        index = (index + 15) & 15;
        a = state[index];
        state[index] = a ^ b ^ d ^ (a << 2) ^ (b << 18) ^ (c << 28);
        return state[index];
    }
};

void base() {
    //cout << "deck의 갯수를 입력하세요 :(1~8) ";
    //cin >> deck;
    //cout << endl;
    deck = 8;
    deckk = 52 * deck;
    cardbase();

    int timet = time(NULL);
    cout << "현재 Seed값은 " << timet << "입니다." << endl;
    Well512Random Seed(timet);

    for (int i = 0; i < deckk; i++) {
        cardh[i][2] = to_string(Seed.Next(1, 10000));
    }

    hip();
}

void game() {
    base();

    //cout << "플레이어의 수를 입력하세요. :(1~8) ";
    //cin >> player;
    //cout << endl;

    string player[8][10][3];
    int player1 = 0, player2 = 0;
    int player11, player21;

    for (int i = 0; i <2; i++) {
         player[i][0][0] = cardh[i][0];
         player[i][0][1] = cardh[i][1];
         player[i][1][0] = cardh[i + 2][0];
         player[i][1][1] = cardh[i + 2][1];
    }

    for (int i = 0; i < 2; i++) {
        if (player[0][i][1] == "T") { player1 = player1+10; }
        else if (player[0][i][1] == "J") { player1 = player1+10; }
        else if (player[0][i][1] == "Q") { player1 = player1+10; }
        else if (player[0][i][1] == "K") { player1 = player1 +10; }
        else if (player[0][i][1] == "A") { player1 = player1 +1; }
        else { player1 = player1 +atoi(player[0][i][1].c_str()); }
    }

    for (int i = 0; i < 2; i++) {
        if (player[1][i][1] == "T") { player2 = player2 +10; }
        else if (player[1][i][1] == "J") { player2 = player2 +10; }
        else if (player[1][i][1] == "Q") { player2 = player2 +10; }
        else if (player[1][i][1] == "K") { player2 = player2 +10; }
        else if (player[1][i][1] == "A") { player2 = player2 +1; }
        else { player2 = player2 +atoi(player[0][i][1].c_str()); }
    }

    int ca1 = 2;
    int ca3 = 2;
    int ca4 = 2;
    while (player1 <= 16) {
        player[0][ca3][0] = cardh[2 + ca1][0];
        player[0][ca3][1] = cardh[2 + ca1][1];
        if (player[0][ca1][1] == "T") { player1 = player1+10; }
        else if (player[0][ca3][1] == "J") { player1 = player1+10; }
        else if (player[0][ca3][1] == "Q") { player1 = player1+10; }
        else if (player[0][ca3][1] == "K") { player1 = player1+10; }
        else if (player[0][ca3][1] == "A") { player1 = player1+1; }
        else { player1 = player1+atoi(player[0][ca3][1].c_str()); }
        ca1++;
        ca3++;
    }

    for (int i = 0; i < 2; i++) {
        cout << "받은 카드는 " << player[1][i][0] << player[1][i][1] << " 입니다." << endl;
    }

    char ca0, ca2;
    

    do {
        do {
            cout << "카드를 더 받으시겠습니까?(y/n)";
            cin >> ca0;
            cout << endl;
            if (ca0 == 'n') { c = 0; }
            else if (ca0 == 'y') { c = 0; }
            else {
                c = 1;
                cout << "잘못 입력하였습니다." << endl;
            }
        } while (c == 1);
        if (ca0 == 'y') {
            player[1][ca4][0] = cardh[2+ca1][0];
            player[1][ca4][1] = cardh[2+ca1][1];
            if (player[1][ca4][1] == "T") { player2 = player2 +10; }
            else if (player[1][ca4][1] == "J") { player2 = player2 +10; }
            else if (player[1][ca4][1] == "Q") { player2 = player2 +10; }
            else if (player[1][ca4][1] == "K") { player2 = player2 +10; }
            else if (player[1][ca4][1] == "A") { player2 = player2 +1; }
            else { player2 = player2 +atoi(player[0][ca4][1].c_str()); }
            ca1++;
            cout << "받은 카드는 " << player[1][ca4][0] << player[1][ca4][1] << " 입니다." << endl;
            ca4++;
        }
    } while (ca0 == 'y');

    for (int i = 0; i < ca4; i++) {
        if (player[1][i][1] == "A") {
            do {
                cout << "카드 A를 11로 사용하시겟습니까?(y/n)";
                cin >> ca2;
                cout << endl;
                if (ca2 == 'n') { c = 0; }
                else if (ca2 == 'y') { c = 0; }
                else {
                    c = 1;
                    cout << "잘못 입력하였습니다." << endl;
                }
            } while (c == 1);
            if (ca2 == 'y') {
                player2 = player2 +10;
            }
        }
    }
    
    cout << "딜러의 카드는 ";
    for (int i = 0; i < ca3; i++) {
        cout << player[0][i][0] << player[0][i][1] << endl;
    }
    cout << " 입니다.";

    player11 = (21 - player1) * (21 - player1);
    player21 = (21 - player2) * (21 - player2);

    if (player11 == player21) {
        cout << "게임에서 비겼습니다." << endl;
    }
    else if (player11 < player21) {
        cout << "게임에서 패배하였습니다." << endl;
    }
    else if (player11 > player21) {
        cout << "게임에서 승리하였습니다." << endl;
    }
    else {
        cout << "게임에 문제가 있습니다." << endl;
    }
}

void main() {
    char a;

    do {
        game();
        cout << endl;
        do {
            cout << "게임을 종료 하시겠습니까?(y/n)";
            cin >> a;
            cout << endl;
            if (a == 'n') { c = 0; }
            else if (a == 'y') { c = 0; }
            else {
                c = 1;
                cout << "잘못 입력하였습니다." << endl;
            }
        } while (c == 1);

    } while (a == 'n');
}
