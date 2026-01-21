#include <stdio.h>
#include <string.h>
#define MAX_ITEMS 100
#define DISCOUNT_THRESHOLD 100.0
#define DISCOUNT_RATE 0.10
#define TAX_RATE 0.07


void displayMenu() {
    printf("=============================================\n");
    printf("Welcome to the Eatery Order Management System\n");
    printf("=============================================\n");
    printf("1. Process a new order\n");
    printf("2. End the day and show summary report\n");
    printf("=============================================\n");
    printf("Enter your choice: ");
}
void processOrder(float *totalRevenue, float *totalDiscounts, int *orderCount) {
    int itemCount = 0;
    float prices[MAX_ITEMS];
    int quantities[MAX_ITEMS];
    char itemNames[MAX_ITEMS][50];
    float subtotal = 0.0;
    char moreItems;

    do{
        printf("Enter item name: ");
        getchar();
        fgets(itemNames[itemCount], 50, stdin);
        itemNames[itemCount][strcspn(itemNames[itemCount], "\n")]=0;

        do{
            printf("Enter item price: ");
            if (scanf("%f", &prices[itemCount]) != 1){
                while(getchar() != '\n');
                prices[itemCount] = -1;
            }
            if(prices[itemCount]<=0){
                printf("Invalid price! Please enter a positive value.\n");
                printf("=============================================\n");
            }
        } while(prices[itemCount]<=0);


        do{
            printf("Enter item quantity: ");
            if (scanf("%d", &quantities[itemCount]) != 1){
                while(getchar() != '\n');
                quantities[itemCount] = -1;
            }
            if(quantities[itemCount]<=0){
                printf("Invalid quantity! Please enter a positive integer.\n");
                printf("==================================================\n");
            }
        } while(quantities[itemCount]<=0);

        subtotal += prices[itemCount] * quantities[itemCount];
        itemCount++;

    
        do{
            printf("Add more items to this order? (y/n): ");
            scanf(" %c", &moreItems);

            if (moreItems>='A' && moreItems<='Z'){
                moreItems+= 'a'-'A';
            }
            if (moreItems !='y' && moreItems !='n'){
                printf("Invalid input! Please enter 'y' or 'n'.\n");
                printf("=======================================\n");

            }
        } while(moreItems !='y' && moreItems !='n');
    
    } while (moreItems == 'y' && itemCount < MAX_ITEMS);


    float discount = 0.0;
    float tax = 0.0;
    if (subtotal > DISCOUNT_THRESHOLD) {
        discount = subtotal * DISCOUNT_RATE;
    }
    tax = (subtotal - discount) * TAX_RATE;
    float total = subtotal - discount + tax;

    printf("========================================\n");
    printf("Items:\n");
    for (int i = 0; i < itemCount; i++) {
        printf("- %s x%d = $%.2f\n",
           itemNames[i],
           quantities[i],
           prices[i] * quantities[i]);
        }
    printf("========================================\n");
    printf("Subtotal: $%.2f\n", subtotal);
    if (discount > 0) {
        printf("Discount: $%.2f\n", discount);
    }
    if (tax > 0) {
        printf("Tax: $%.2f\n", tax);
    }
    printf("Total Payable: $%.2f\n", total);

    *totalRevenue += total;
    *totalDiscounts += discount;
    (*orderCount)++;
}

int main() {
    float totalRevenue = 0.0;
    float totalDiscounts = 0.0;
    int orderCount = 0;
    int choice;

    do{
        displayMenu();
        scanf("%d", &choice);
        switch (choice) {
            case 1:
                processOrder(&totalRevenue, &totalDiscounts, &orderCount);
                break;
            case 2:
                printf("\n========================================\n");
                printf("End of Day Summary Report:\n");
                printf("========================================\n");
                printf("Orders Processed: %d\n", orderCount);
                printf("Total Revenue: $%.2f\n", totalRevenue);
                printf("Total Discounts Given: $%.2f\n", totalDiscounts);
                if (orderCount > 0) {
                    printf("Average Order Value: $%.2f\n", totalRevenue / orderCount);
                    printf("========================================\n");
                } else {
                    printf("========================================\n");
                    printf("No orders processed today!\n");
                    printf("========================================\n");
                }
                break;
            default:
                printf("Invalid choice! Please try again!\n");
        }
    } while (choice != 2);
    return 0;
}
