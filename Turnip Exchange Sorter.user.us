// ==UserScript==
// @name         Turnip Exchange Sorter
// @namespace    com.timatlee.turnip.exchange.filter
// @version      0.0.1
// @description  Sorts the Turnip Exchange by sale price (and queue length?)
// @author       Tim AtLee
// @match        https://turnip.exchange/islands
// @require      https://code.jquery.com/jquery-3.5.1.min.js
// @run-at       document-end
// ==/UserScript==

(function() {
    'use strict';

    let bells_regexp = new RegExp(/(\d{2,3}) Bells/);
    let queue_regexp = new RegExp(/Waiting: (\d{1,3})\/(\d{1,3})/);

    console.log("Hello.  Starting Turnip Exchange Sorter");
    console.log("jQuery version I think loaded: ", $().jquery);

    let ifReplaced = false;
    // Periodically check the page for cards.
    let checker = setInterval(() => {
        // If we have found cards, stop polling the page.
        if (ifReplaced) {
            clearInterval(checker);
            return;
        }
        // Try to find price cards
        let cards = $("div[data-turnip-code]");
        // If we have cards on the screen..
        if (cards.length > 3) {
            // We can start to do work. First, stop checking.
            ifReplaced = true;
            // Iterate over the cards
            cards.each(function() {
                // $(this) is an individual card.

                // Try to find the price node
                let PriceNode = $(this).find("div > p").filter(function() {
                    return $(this).text().match(bells_regexp);
                });

                // Try to find the waiting queue line
                let QueueNode = $(this).find("div > p").filter(function() {
                    return $(this).text().match(queue_regexp);
                });

                let price = parseInt(PriceNode.text().match(bells_regexp)[1]);
                let queue_now = parseInt(QueueNode.text().match(queue_regexp)[1]);
                let queue_max = parseInt(QueueNode.text().match(queue_regexp)[2]);

                $(this).attr('data-price', price);
                $(this).attr('data-queue', (queue_now/queue_max));
            });
        }

        // Sort the cards by price.
        cards.sort(function (a, b) {
            var contentA =parseInt( $(a).attr('data-price'));
            var contentB =parseInt( $(b).attr('data-price'));
            return (contentA < contentB) ? 1 : (contentA > contentB) ? -1 : 0;
        }).appendTo(cards.parent());
    }, 1000);
})();
