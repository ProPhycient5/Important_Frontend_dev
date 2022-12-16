- The following code is the development of UI with functionalities on Frontend for online card payment.
- It has important three inputs like card no., expiry date and CVV no.
- It includes validation for all the fields.


```

import React from "react";
import { useState } from "react";
import Image from "next/image";
import Link from "next/link";
import Card from "../../public/icons/card.svg";
import Encryption from "../../public/icons/encryption.svg";
import BannerImage from "../../components/BannerImage.jsx";
import VisaLogo from "../../public/icons/visa-logo.svg";
import MasterLogo from "../../public/icons/master-logo.svg";
import SuccessGIF from "../../public/icons/success-gif.json";
import Lottie from "lottie-react";
import cx from "classnames";
import hasValidValues from "../../utils/index.js";

const PaymentSection = () => {
  const [showSuccess, setShowSuccess] = useState(false);

  return (
    <div className="w-full bg-unplex-black min-h-screen flex flex-col justify-start items-center">
      {!showSuccess ? (
        <CardSection setShowSuccess={setShowSuccess} />
      ) : (
        <SuccessMsg />
      )}
    </div>
  );
};

export default PaymentSection;

const CardSection = ({ setShowSuccess }) => {
  //dummyData has to be replaced by dynamic data.
  const dummyData = {
    Name: "Dad",
    Mobile: "+919740495689",
    Amount: "100.00",
    Fee: "1.67",
    ReceivedAmount: "8000",
    SelectedCurrency: "₹",
  };

  const [cardData, setCardData] = useState({
    cardNo: "",
    expiryDate: "",
    cvv: "",
  });
  const [errorMsg, setErrorMsg] = useState("");
  const [errorSignal, setErrorSignal] = useState({
    cardNo: false,
    expiryDate: false,
    cvv: false,
  });

  function assignAllFalse(obj) {
    Object.keys(obj).forEach((key) => {
      obj[key] = false;
    });
    return obj;
  }

  function assignFalseExceptOne(obj, selectedKey) {
    Object.keys(obj).forEach((key) => {
      if (key != selectedKey) obj[key] = false;
    });
    return obj;
  }

  function validation(obj) {
    const tempObj = errorSignal;
    if (hasValidValues(obj)) {
      //if all inputs have values, then below cond is for deep validation
      let extractedCardNum = obj?.cardNo.match(/\d+/g).join("");
      let extractedDateNum = obj?.expiryDate.replace(/\D/g, "");
      if (extractedCardNum.length !== 16) {
        setErrorMsg("Card numbers have to be 16 digits");
        setErrorSignal({ ...assignFalseExceptOne(tempObj, "cardNo") });
        setErrorSignal((prev) => ({ ...prev, cardNo: true }));
        return false;
      } else if (obj?.cvv.trim().length !== 3) {
        setErrorMsg("CVV numbers have to be 3 digits");
        setErrorSignal({ ...assignFalseExceptOne(tempObj, "cvv") });
        setErrorSignal((prev) => ({ ...prev, cvv: true }));
        return false;
      } else if (extractedDateNum.length !== 4) {
        setErrorMsg("Expiry date is incomplete");
        setErrorSignal({ ...assignFalseExceptOne(tempObj, "expiryDate") });
        setErrorSignal((prev) => ({ ...prev, expiryDate: true }));
        return false;
      } else {
        //here every cond is passed, data is validated
        setErrorSignal({ ...assignAllFalse(tempObj) });
        setErrorMsg("");
        return true;
      }
    } else if (!obj?.cardNo) {
      setErrorSignal({ ...assignFalseExceptOne(tempObj, "cardNo") });
      setErrorSignal((prev) => ({ ...prev, cardNo: true }));
      setErrorMsg("Enter the required card numbers!");
      return false;
    } else if (!obj?.expiryDate) {
      setErrorSignal({ ...assignFalseExceptOne(tempObj, "expiryDate") });
      setErrorSignal((prev) => ({ ...prev, expiryDate: true }));
      setErrorMsg("Enter the required expiry date!");
      return false;
    } else if (!obj?.cvv) {
      setErrorSignal({ ...assignFalseExceptOne(tempObj, "cvv") });
      setErrorSignal((prev) => ({ ...prev, cvv: true }));
      setErrorMsg("Enter the required CVV number!");
      return false;
    }
  }

  const expiryFormat = (value) => {
    const expDate = value;
    const expDateFormatter =
      expDate.replace(/\//g, "").substring(0, 2) +
      (expDate.length > 2 ? "/" : "") +
      expDate.replace(/\//g, "").substring(2, 4);
    return expDateFormatter;
  };

  const cardNumberFormat = (value) => {
    const v = value.replace(/[^0-9]/gi, "").substr(0, 16);
    const parts = [];
    for (let i = 0; i < v.length; i += 4) {
      parts.push(v.substr(i, 4));
    }
    return parts.length > 1 ? parts.join("-") : value;
  };

  const validKeyForPayment = ["0","1","2","3","4","5","6","7","8","9","Backspace"];

  console.log("cardData -->", cardData);
  return (
    <>
      <Header />
      <div className="text-white w-full max-w-lg px-3 py-0 sm:py-4 mt-20">
        <BannerImage conversationDetails={dummyData} />
      </div>
      {/* main content */}
      <div className="text-center font-sans text-white flex justify-start items-center flex-col w-full max-w-lg px-3 sm:my-4">
        <div className="bg-black w-full rounded-xl flex flex-col items-start p-4">
          <div className="w-full flex flex-row justify-between items-center mb-4">
            <div className="text-base text-gray-300">Your Debit Card</div>
            <div className="flex flex-row items-center">
              <Image
                className="mr-3"
                width="auto"
                height="auto"
                src={VisaLogo}
                alt="visa-logo"
              />
              <Image
                src={MasterLogo}
                width="auto"
                height="auto"
                alt="master-logo"
              />
            </div>
          </div>

          <div
            className={cx(
              "p-1.5 mb-4 w-full rounded-md flex flex-col justify-between items-start",
              {
                "border border-gray-600": errorSignal?.cardNo === false,
              },
              {
                "border border-red-500": errorSignal?.cardNo === true,
              }
            )}
          >
            <div className="text-xs text-welcome ml-1.5 mb-0.5">
              Card Number
            </div>
            <input
              className="w-full px-1.5 h-6 border-none bg-black text-sm"
              type="text"
              placeholder="XXXX-XXXX-XXXX-XXXX"
              value={cardData?.cardNo}
              onChange={(e) => {
                setCardData((prevData) => ({
                  ...prevData,
                  cardNo: cardNumberFormat(e.target.value),
                }));
              }}
              onKeyDown={(e) => {
                if (!validKeyForPayment.includes(e.key)) {
                  e.preventDefault();
                }
              }}
            />
          </div>
          {errorSignal?.cardNo && (
            <div className="text-xs w-full text-left px-1.5 -mt-2 mb-2 text-red-500">
              {errorMsg}
            </div>
          )}

          <div className="mb-2 w-full flex flex-row justify-between">
            <div
              className={cx(
                "h-full p-1.5 w-48 flex flex-col justify-between items-start rounded-md",
                {
                  "border border-gray-600": errorSignal?.expiryDate === false,
                },
                {
                  "border border-red-500": errorSignal?.expiryDate === true,
                }
              )}
            >
              <div className="text-xs text-welcome ml-1.5 mb-0.5">
                Expiry Date
              </div>
              <input
                className="w-full px-1.5 h-6 border-none bg-black text-sm"
                type="text"
                placeholder="MM/YY"
                onKeyDown={(e) => {
                  if (!validKeyForPayment.includes(e.key)) {
                    e.preventDefault();
                  }
                }}
                value={cardData?.expiryDate}
                onChange={(e) => {
                  setCardData((prevData) => ({
                    ...prevData,
                    expiryDate: expiryFormat(e.target.value),
                  }));
                }}
              />
            </div>
            <div
              className={cx(
                "h-full p-1.5 w-48 flex flex-col justify-between items-start rounded-md",
                {
                  "border border-gray-600": errorSignal?.cvv === false,
                },
                {
                  "border border-red-500": errorSignal?.cvv === true,
                }
              )}
            >
              <div className="text-xs text-welcome ml-1.5 mb-0.5">CVV</div>
              <input
                className="w-full px-1.5 h-6 border-none bg-black text-sm"
                type="password"
                maxLength={3}
                placeholder="000"
                pattern="[0-9][0-9][0-9]"
                defaultValue={cardData?.cvv}
                onKeyDown={(e) => {
                  if (!validKeyForPayment.includes(e.key)) {
                    e.preventDefault();
                  }
                }}
                onChange={(e) => {
                  setCardData((prevData) => ({
                    ...prevData,
                    cvv: e.target.value,
                  }));
                }}
              />
            </div>
          </div>

          <div className="w-full flex flex-row justify-between">
            <div className="w-48">
              {errorSignal?.expiryDate && (
                <div className="text-xs w-full px-1.5 text-left text-red-500">
                  {errorMsg}
                </div>
              )}
            </div>
            <div className="w-48">
              {errorSignal?.cvv && (
                <div className="text-xs w-full px-1.5 text-left text-red-500">
                  {errorMsg}
                </div>
              )}
            </div>
          </div>
        </div>
        <div className="flex flex-row items-center justify-center px-3 my-6 w-full">
          <Image className="mr-2" src={Encryption} alt="encryption" />
          <div className="fs_11">
            Your information at safe and encrypted. AE256 encryption
          </div>
        </div>

        <button
          onClick={() => {
            if (validation(cardData)) setShowSuccess(true);
          }}
          className="bg-welcome pop_up_effect hover:bg-opacity-90 my-2 w-full h-12 rounded-xl flex justify-center items-center text-base"
        >
          Link Card & Pay
        </button>
      </div>
    </>
  );
};

const SuccessMsg = () => {
  return (
    <div className="flex flex-col justify-center items-center w-full pb-3 px-3">
      {/* <div className="h-auto sm:h-64"> */}
      <Lottie animationData={SuccessGIF} loop={true} />
      {/* </div> */}
      <div className="flex flex-col justify-center items-center text-gray-400">
        <div className="mb-8 text-base text-center">
          The amount will be debited from your card very soon...
        </div>
        <div className="text-sm">Back to Whatsapp</div>
        <Link className="text-xs text-blue-500" href="/">
          www.whatsapp.com/unplex/claims.com
        </Link>
      </div>
    </div>
  );
};

const Header = () => {
  return (
    <div className="h-20 w-full flex flex-col justify-between items-center font-sans fixed top-0 z-10 bg-unplex-black">
      <div className="w-full max-w-lg text-gray-300 flex flex-row justify-between items-center p-4">
        <div className="flex flex-row item-center justify-center">
          <Image className="mr-4 h-10 w-10" src={Card} alt="card" />
          <p className="font-lg text-gray-300 mt-2">Add card</p>
        </div>
        <div className="py-2 px-5 bg-gray-500 bg-opacity-40 rounded-3xl">
          3/3
        </div>
      </div>

      <div className="w-full h-1 flex flex-row">
        <div className="w-full bg-welcome h-full"></div>
        {/* <div className="w-1/12 bg-gray-400 h-full"></div> */}
      </div>
    </div>
  );
};

```
