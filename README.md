# real-estate-calculator
import { useState } from "react";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";

export default function RealEstateCalculator() {
  const [inputs, setInputs] = useState({
    purchasePrice: 250000,
    monthlyRent: 2500,
    monthlyExpenses: 1000,
    vacancyRate: 5,
    appreciationRate: 3,
    holdingPeriod: 10,
  });

  const [results, setResults] = useState(null);

  const handleChange = (e) => {
    setInputs({ ...inputs, [e.target.name]: parseFloat(e.target.value) });
  };

  const calculate = () => {
    const {
      purchasePrice,
      monthlyRent,
      monthlyExpenses,
      vacancyRate,
      appreciationRate,
      holdingPeriod,
    } = inputs;

    const effectiveRent = monthlyRent * (1 - vacancyRate / 100);
    const monthlyCashFlow = effectiveRent - monthlyExpenses;
    const annualCashFlow = monthlyCashFlow * 12;
    const noi = annualCashFlow;
    const capRate = (noi / purchasePrice) * 100;
    const roi = (annualCashFlow / purchasePrice) * 100;
    const futureValue = purchasePrice * Math.pow(1 + appreciationRate / 100, holdingPeriod);
    const totalCashFlow = annualCashFlow * holdingPeriod;
    const equityGained = futureValue - purchasePrice;
    const totalProfit = totalCashFlow + equityGained;

    setResults({
      monthlyCashFlow,
      annualCashFlow,
      noi,
      capRate,
      roi,
      futureValue,
      totalCashFlow,
      equityGained,
      totalProfit,
    });
  };

  return (
    <div className="grid gap-4 max-w-xl mx-auto p-4">
      <h1 className="text-2xl font-bold">Real Estate Investment Calculator</h1>
      <Card>
        <CardContent className="grid gap-2 p-4">
          {[
            { label: "Purchase Price ($)", name: "purchasePrice" },
            { label: "Monthly Rent ($)", name: "monthlyRent" },
            { label: "Monthly Expenses ($)", name: "monthlyExpenses" },
            { label: "Vacancy Rate (%)", name: "vacancyRate" },
            { label: "Appreciation Rate (%)", name: "appreciationRate" },
            { label: "Holding Period (Years)", name: "holdingPeriod" },
          ].map((field) => (
            <div key={field.name}>
              <label className="block text-sm font-medium mb-1">{field.label}</label>
              <Input
                type="number"
                name={field.name}
                value={inputs[field.name]}
                onChange={handleChange}
              />
            </div>
          ))}
          <Button onClick={calculate}>Calculate</Button>
        </CardContent>
      </Card>

      {results && (
        <Card>
          <CardContent className="grid gap-2 p-4">
            <h2 className="text-xl font-semibold">Results</h2>
            <div>Monthly Cash Flow: ${results.monthlyCashFlow.toFixed(2)}</div>
            <div>Annual Cash Flow: ${results.annualCashFlow.toFixed(2)}</div>
            <div>NOI: ${results.noi.toFixed(2)}</div>
            <div>Cap Rate: {results.capRate.toFixed(2)}%</div>
            <div>Cash-on-Cash ROI: {results.roi.toFixed(2)}%</div>
            <div>Future Property Value: ${results.futureValue.toFixed(2)}</div>
            <div>Total Cash Flow Over {inputs.holdingPeriod} Years: ${results.totalCashFlow.toFixed(2)}</div>
            <div>Equity Gained: ${results.equityGained.toFixed(2)}</div>
            <div>Total Profit: ${results.totalProfit.toFixed(2)}</div>
          </CardContent>
        </Card>
      )}
    </div>
  );
}
