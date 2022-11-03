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

## бага в пуше объетка 
```
      get content() {
        const returnContent: Array<{
          consigneeName: string;
          consigneeInn: string;
        }> = [];

        const inputContent: {
          consigneeName: string;
          consigneeInn: string;
        } = {
          consigneeName: "",
          consigneeInn: "",
        };

        if(seriesOrder && seriesOrder.deliveryPoints.length) {
          Object.keys(seriesOrder.deliveryPoints).forEach((item) => {
            if (
              seriesOrder.deliveryPoints[item] &&
              seriesOrder.deliveryPoints[item].consigneeName &&
              seriesOrder.deliveryPoints[item].consigneeInn
            ) {
              console.log(seriesOrder.deliveryPoints[item]);
              inputContent.consigneeName = seriesOrder.deliveryPoints[item].consigneeName;
              inputContent.consigneeInn = seriesOrder.deliveryPoints[item].consigneeInn;
              console.log(inputContent);
              returnContent.push(inputContent);
              console.log(returnContent);
             
            }
          });
        }

        return returnContent;
      },
```
