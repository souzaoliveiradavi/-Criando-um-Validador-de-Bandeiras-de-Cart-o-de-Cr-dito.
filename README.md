# -Criando-um-Validador-de-Bandeiras-de-Cartao-de-Credito.
function getCardType(number) {
    const cardPatterns = {
        visa: /^4[0-9]{12}(?:[0-9]{3})?$/,
        mastercard: /^5[1-5][0-9]{14}$/,
        amex: /^3[47][0-9]{13}$/,
        discover: /^6(?:011|5[0-9]{2})[0-9]{12}$/,
        diners: /^3(?:0[0-5]|[68][0-9])[0-9]{11}$/,
        jcb: /^(?:2131|1800|35\d{3})\d{11}$/
    };

    for (const [card, pattern] of Object.entries(cardPatterns)) {
        if (pattern.test(number)) {
            return card;
        }
    }
    return 'unknown';
}

function validateCardNumber(number) {
    const regex = new RegExp("^[0-9]{13,19}$");
    if (!regex.test(number)) return false;

    let sum = 0;
    let shouldDouble = false;
    for (let i = number.length - 1; i >= 0; i--) {
        let digit = parseInt(number.charAt(i), 10);

        if (shouldDouble) {
            digit *= 2;
            if (digit > 9) digit -= 9;
        }

        sum += digit;
        shouldDouble = !shouldDouble;
    }

    return sum % 10 === 0;
}

function validateCreditCard(number) {
    if (typeof number !== 'string') {
        throw new Error('Card number must be a string');
    }

    const cardType = getCardType(number);
    const isValid = validateCardNumber(number);

    return {
        cardType,
        isValid
    };
}

// Exemplo de uso
try {
    const cardNumber = '4111111111111111';
    const result = validateCreditCard(cardNumber);
    console.log(`Card Type: ${result.cardType}, Valid: ${result.isValid}`);
} catch (error) {
    console.error(error.message);
}
