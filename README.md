# notes
My notes

## TNQRC 

# сторик - ???

function of get time
```
function convertDate(date) {
  const newDate = new Date(date);

  if (newDate.getDate()) {
    const month = newDate.getMonth() + 1;
    const day = newDate.getDate();
    const year = newDate.getFullYear();

    return day + "." + month + "." + year;
  } else {
    return "";
  }
}
```

i expore type for response
```
export type ResponseGetAboutOrder_New = {
  productionDivisionName?: string;
  shippingOrderNumber?: string;
  shippingOrderDate?: string;
  deliveryPoints: Array<IAboutOrderDelivery_New>;
};

export type IAboutOrderDelivery_New = {
  objectName?: string;
  consigneeName?: string;
  consigneeInn?: string;
};
```
