'use client'

import { useState, useEffect } from 'react'
import { Button } from "@/components/ui/button"
import { Progress } from "@/components/ui/progress"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"

export default function CoinTapperGame() {
  const [coins, setCoins] = useState(0)
  const [coinsPerTap, setCoinsPerTap] = useState(1)
  const [coinsPerHour, setCoinsPerHour] = useState(0)
  const [boostCost, setBoostCost] = useState(10)
  const [taskProgress, setTaskProgress] = useState(0)

  useEffect(() => {
    const timer = setInterval(() => {
      setCoins(prevCoins => prevCoins + coinsPerHour / 3600)
    }, 1000)

    return () => clearInterval(timer)
  }, [coinsPerHour])

  const handleTap = () => {
    setCoins(prevCoins => prevCoins + coinsPerTap)
    setTaskProgress(prevProgress => Math.min(prevProgress + 5, 100))
  }

  const buyBoost = () => {
    if (coins >= boostCost) {
      setCoins(prevCoins => prevCoins - boostCost)
      setCoinsPerTap(prevCoinsPerTap => prevCoinsPerTap + 1)
      setCoinsPerHour(prevCoinsPerHour => prevCoinsPerHour + 10)
      setBoostCost(prevCost => Math.floor(prevCost * 1.5))
    }
  }

  const completeTask = () => {
    if (taskProgress === 100) {
      setCoins(prevCoins => prevCoins + 100)
      setTaskProgress(0)
    }
  }

  return (
    <Card className="w-full max-w-md mx-auto">
      <CardHeader>
        <CardTitle className="text-2xl font-bold text-center">Coin Tapper</CardTitle>
      </CardHeader>
      <CardContent className="space-y-4">
        <div className="text-center">
          <p className="text-4xl font-bold">{Math.floor(coins)}</p>
          <p className="text-sm text-muted-foreground">Coins</p>
        </div>
        <div className="text-center">
          <p className="text-xl font-semibold">{coinsPerTap}</p>
          <p className="text-sm text-muted-foreground">Coins per tap</p>
        </div>
        <div className="text-center">
          <p className="text-xl font-semibold">{coinsPerHour}</p>
          <p className="text-sm text-muted-foreground">Coins per hour</p>
        </div>
        <Button className="w-full h-24 text-2xl" onClick={handleTap}>
          Tap to Earn
        </Button>
        <Button className="w-full" onClick={buyBoost} disabled={coins < boostCost}>
          Buy Boost (Cost: {Math.floor(boostCost)})
        </Button>
        <div>
          <p className="text-sm font-semibold mb-2">Task Progress</p>
          <Progress value={taskProgress} className="w-full" />
        </div>
        <Button className="w-full" onClick={completeTask} disabled={taskProgress < 100}>
          Complete Task (Reward: 100 coins)
        </Button>
      </CardContent>
    </Card>
  )
}
