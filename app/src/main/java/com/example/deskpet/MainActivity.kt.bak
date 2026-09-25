package com.example.deskpet

import android.Manifest
import android.app.Activity
import android.content.Intent
import android.content.pm.PackageManager
import android.net.Uri
import android.os.Build
import android.os.Bundle
import android.provider.Settings
import android.widget.Button
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import com.example.deskpet.service.OverlayService

class MainActivity : Activity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(buildUi())
        requestNotificationPermissionIfNeeded()
    }

    private fun buildUi(): LinearLayout {
        val layout = LinearLayout(this).apply {
            orientation = LinearLayout.VERTICAL
            setPadding(48, 96, 48, 48)
        }

        layout.addView(TextView(this).apply {
            text = "AI-Live-Overflow 桌宠"
            textSize = 22f
        })
        layout.addView(TextView(this).apply {
            text = "启动后小生物会蹲在屏幕上所有 App 上方。"
            textSize = 14f
            setPadding(0, 16, 0, 32)
        })

        layout.addView(Button(this).apply {
            text = "启动桌宠"
            setOnClickListener { ensureOverlayPermissionAndStart() }
        })

        layout.addView(Button(this).apply {
            text = "停止桌宠"
            setOnClickListener {
                stopService(Intent(this@MainActivity, OverlayService::class.java))
                Toast.makeText(this@MainActivity, "已停止", Toast.LENGTH_SHORT).show()
            }
        })

        return layout
    }

    private fun requestNotificationPermissionIfNeeded() {
        if (Build.VERSION.SDK_INT >= 33) {
            if (checkSelfPermission(Manifest.permission.POST_NOTIFICATIONS)
                != PackageManager.PERMISSION_GRANTED
            ) {
                requestPermissions(arrayOf(Manifest.permission.POST_NOTIFICATIONS), 100)
            }
        }
    }

    private fun ensureOverlayPermissionAndStart() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M && !Settings.canDrawOverlays(this)) {
            Toast.makeText(this, "请先授予「显示在其他应用上层」权限", Toast.LENGTH_LONG).show()
            startActivity(
                Intent(
                    Settings.ACTION_MANAGE_OVERLAY_PERMISSION,
                    Uri.parse("package:$packageName")
                )
            )
            return
        }
        startPet()
    }

    private fun startPet() {
        val intent = Intent(this, OverlayService::class.java)
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            startForegroundService(intent)
        } else {
            startService(intent)
        }
        Toast.makeText(this, "桌宠已蹲在屏幕上啦", Toast.LENGTH_SHORT).show()
        finish()
    }
}
