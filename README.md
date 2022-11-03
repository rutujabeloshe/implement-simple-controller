# implement-simple-controller
import sys


class Bank:
    def __init__(self):
        self.accountbook = {'123-456-789': 15, '135-791-113': 10, '246-810-121': 86}
        self.cardPIN = {'1111 1111 1111 1111': 1357, '2222 2222 2222 2222': 2468, '3333 3333 3333 3333': 2580}
        self.cardNumberAccount = {'1111 1111 1111 1111': ['123-456-789'],
                                  '2222 2222 2222 2222': ['135-791-113', '246-810-121'],
                                  '3333 3333 3333 3333': []}

    def isValidCard(self, cardNumber):
        if cardNumber in self.cardPIN:
            return True
        return False

    def checkPIN(self, cardNumber, PIN):
        if self.cardPIN[cardNumber] == PIN:
            return True
        return False

    def getAccounts(self, cardNumber):
        return self.cardNumberAccount[cardNumber]

    def checkBalance(self, account):
        return self.accountbook[account]

    def deposit(self, account, amount):
        self.accountbook[account] += amount
        return self.accountbook[account]

    def withdraw(self, account, amount):
        if self.accountbook[account] < amount:
            return False
        self.accountbook[account] -= amount
        return self.accountbook[account]


class ATM:
    def __init__(self):
        self.cardNumber = ''
        self.account = ''
        self.bank = Bank()
        self.modes = ['See Balance', 'Deposit', 'Withdraw']
        self.startNewWork()

    def startNewWork(self):
        self.cardNumber = ''
        self.account = ''
        print('Please insert your card.')
        cardNumber = sys.stdin.readline().strip()
        checkResult = self.bank.isValidCard(cardNumber)
        if checkResult:
            self.cardNumber = cardNumber
            return self.getPINNumber()
        print('Invalid Card. Please use different card.')
        return self.startNewWork()

    def getPINNumber(self):
        print('Please enter your PIN number.')
        PINNumber = int(sys.stdin.readline().strip())
        checkResult = self.bank.checkPIN(self.cardNumber, PINNumber)
        if checkResult:
            accounts = self.bank.getAccounts(self.cardNumber)
            if len(accounts) == 0:
                print('There is no account connected with this card. Please visit your bank.\n')
                return self.startNewWork()
            return self.selectAccount(accounts)
        print('Wrong PIN number.')
        return self.getPINNumber()

    def selectAccount(self, accounts):
        print('Please select your account you want to progress transaction.')
        for i in range(len(accounts)):
            print('{}: {}'.format(i+1, accounts[i]))
        while True:
            accountIdx = int(sys.stdin.readline().strip())
            if len(accounts) >= accountIdx > 0:
                break
            print('Please select from list above.')

        self.account = accounts[accountIdx - 1]
        return self.selectMode()

    def selectMode(self):
        print('Please select transaction you want to proceed.')
        for i in range(len(self.modes)):
            print('{}: {}'.format(i+1, self.modes[i]))
        while True:
            modeIdx = int(sys.stdin.readline().strip())
            if len(self.modes) >= modeIdx > 0:
                break
            print('Please select from list above.')

        if modeIdx == 1:
            return self.seeBalance()
        if modeIdx == 2:
            return self.deposit()
        if modeIdx == 3:
            return self.withdraw()
        print('Error occurred. Restarting transaction...')
        return self.startNewWork()

    def seeBalance(self):
        balance = self.bank.checkBalance(self.account)
        print('Your balance: ${}'.format(balance))
        print('End of transaction.\n')
        return self.startNewWork()

    def deposit(self):
        print('Please enter amount of money you want to deposit.')
        amount = int(sys.stdin.readline().strip())
        result = self.bank.deposit(self.account, amount)
        print('Your transaction has done successfully.\nYour current balance: ${}'.format(result))
        print('End of transaction.\n')
        return self.startNewWork()

    def withdraw(self):
        print('Please enter amount of money you want to withdraw.')
        amount = int(sys.stdin.readline().strip())
        result = self.bank.withdraw(self.account, amount)
        if result is False:
            print('Your balance is not enough to withdraw. Please check your balance first.\n')
        else:
            print('Your transaction has done successfully.\nYour current balance: ${}'.format(result))
            print('End of transaction.\n')
        return self.startNewWork()


def main():
    ATM()


main()
@rutujabeloshe
 
Add heading 
